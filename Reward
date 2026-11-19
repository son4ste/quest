// Automatizado por @Son4ste
delete window.$;
let wpRequire = webpackChunkdiscord_app.push([[Symbol()], {}, r => r]);
webpackChunkdiscord_app.pop();

let ApplicationStreamingStore = Object.values(wpRequire.c).find(x => x?.exports?.A?.__proto__?.getStreamerActiveStreamMetadata).exports.A;
let RunningGameStore = Object.values(wpRequire.c).find(x => x?.exports?.Ay?.getRunningGames).exports.Ay;
let QuestsStore = Object.values(wpRequire.c).find(x => x?.exports?.A?.__proto__?.getQuest).exports.A;
let ChannelStore = Object.values(wpRequire.c).find(x => x?.exports?.A?.__proto__?.getAllThreadsForParent).exports.A;
let GuildChannelStore = Object.values(wpRequire.c).find(x => x?.exports?.A?.__proto__?.getSFWDefaultChannel).exports.A;
let FluxDispatcher = Object.values(wpRequire.c).find(x => x?.exports?.h?.__proto__?.flushWaitQueue).exports.h;
let api = Object.values(wpRequire.c).find(x => x?.exports?.Bo?.get).exports.Bo;

const supportedTasks = ["WATCH_VIDEO", "PLAY_ON_DESKTOP", "STREAM_ON_DESKTOP", "PLAY_ACTIVITY", "WATCH_VIDEO_ON_MOBILE"]
let quests = [...QuestsStore.quests.values()].filter(x => x.userStatus?.enrolledAt && !x.userStatus?.completedAt && new Date(x.config.expiresAt).getTime() > Date.now() && supportedTasks.find(y => Object.keys((x.config.taskConfig ?? x.config.taskConfigV2).tasks).includes(y)))
let isApp = typeof DiscordNative !== "undefined"

if(quests.length === 0) {
	console.log("You don't have any uncompleted quests!")
} else {
	const taskOf = quest => {
		const taskConfig = quest.config.taskConfig ?? quest.config.taskConfigV2
		const taskName = supportedTasks.find(x => taskConfig.tasks[x] != null)
		return { taskName, taskData: taskConfig.tasks[taskName] }
	}
	const questName = quest => quest.config.messages.questName
	const progressOf = (quest, taskName) => {
		const live = QuestsStore.quests.get(quest.id) ?? quest
		if(live.config.configVersion === 1 && live.userStatus?.streamProgressSeconds != null) return live.userStatus.streamProgressSeconds
		return live.userStatus?.progress?.[taskName]?.value ?? 0
	}

	let remaining = quests.length
	const finish = quest => {
		remaining--
		console.log("Quest completed!", questName(quest))
		if(remaining === 0) console.log("All quests completed! You can claim the rewards now.")
	}

	const trackProgress = (quest, taskName, secondsNeeded, onDone) => {
		const interval = setInterval(() => {
			const progress = progressOf(quest, taskName)
			console.log(`Quest progress (${questName(quest)}): ${Math.floor(progress)}/${secondsNeeded}`)
			if(progress >= secondsNeeded) {
				clearInterval(interval)
				onDone?.()
				finish(quest)
			}
		}, 5000)
	}

	console.log(`Running ${quests.length} quest(s) at the same time.`)

	for(const quest of quests.filter(q => ["WATCH_VIDEO", "WATCH_VIDEO_ON_MOBILE"].includes(taskOf(q).taskName))) {
		const { taskName, taskData } = taskOf(quest)
		const secondsNeeded = taskData.target
		let secondsDone = progressOf(quest, taskName)
		const speed = 7
		;(async () => {
			let completed = false
			while(secondsDone < secondsNeeded) {
				const remainingSeconds = Math.min(speed, secondsNeeded - secondsDone)
				await new Promise(resolve => setTimeout(resolve, remainingSeconds * 1000))

				const timestamp = secondsDone + speed
				const res = await api.post({url: `/quests/${quest.id}/video-progress`, body: {timestamp: Math.min(secondsNeeded, timestamp + Math.random())}})
				completed = res.body.completed_at != null
				secondsDone = Math.min(secondsNeeded, timestamp)
			}
			if(!completed) {
				await api.post({url: `/quests/${quest.id}/video-progress`, body: {timestamp: secondsNeeded}})
			}
			finish(quest)
		})()
		console.log(`Spoofing video for ${questName(quest)}.`)
	}

	for(const quest of quests.filter(q => taskOf(q).taskName === "PLAY_ACTIVITY")) {
		const { taskData } = taskOf(quest)
		const secondsNeeded = taskData.target
		const channelId = ChannelStore.getSortedPrivateChannels()[0]?.id ?? Object.values(GuildChannelStore.getAllGuilds()).find(x => x != null && x.VOCAL.length > 0).VOCAL[0].channel.id
		const streamKey = `call:${channelId}:1`
		;(async () => {
			console.log("Completing quest", questName(quest))
			while(true) {
				const res = await api.post({url: `/quests/${quest.id}/heartbeat`, body: {stream_key: streamKey, terminal: false}})
				const progress = res.body.progress.PLAY_ACTIVITY.value
				console.log(`Quest progress (${questName(quest)}): ${Math.floor(progress)}/${secondsNeeded}`)

				await new Promise(resolve => setTimeout(resolve, 20 * 1000))

				if(progress >= secondsNeeded) {
					await api.post({url: `/quests/${quest.id}/heartbeat`, body: {stream_key: streamKey, terminal: true}})
					break
				}
			}
			finish(quest)
		})()
	}

	const playQuests = quests.filter(q => taskOf(q).taskName === "PLAY_ON_DESKTOP")
	if(playQuests.length) {
		if(!isApp) {
			console.log("PLAY_ON_DESKTOP quests need the discord desktop app, skipping:", playQuests.map(questName).join(", "))
			remaining -= playQuests.length
		} else {
			Promise.all(playQuests.map(async quest => {
				const { taskData } = taskOf(quest)
				const applicationId = quest.config.application?.id ?? taskData.applications?.[0]?.id
				const res = await api.get({url: `/applications/public?application_ids=${applicationId}`})
				const appData = res.body[0]
				const exeName = appData.executables?.find(x => x.os === "win32")?.name?.replace(">","") ?? appData.name.replace(/[\/\\:*?"<>|]/g, "")
				const pid = Math.floor(Math.random() * 30000) + 1000

				return {
					quest,
					appData,
					fakeGame: {
						cmdLine: `C:\\Program Files\\${appData.name}\\${exeName}`,
						exeName,
						exePath: `c:/program files/${appData.name.toLowerCase()}/${exeName}`,
						hidden: false,
						isLauncher: false,
						id: applicationId,
						name: appData.name,
						pid: pid,
						pidPath: [pid],
						processName: appData.name,
						start: Date.now(),
					}
				}
			})).then(entries => {
				const realGames = RunningGameStore.getRunningGames()
				const fakeGames = entries.map(x => x.fakeGame)
				const realGetRunningGames = RunningGameStore.getRunningGames
				const realGetGameForPID = RunningGameStore.getGameForPID
				RunningGameStore.getRunningGames = () => fakeGames
				RunningGameStore.getGameForPID = pid => fakeGames.find(x => x.pid === pid)
				FluxDispatcher.dispatch({type: "RUNNING_GAMES_CHANGE", removed: realGames, added: fakeGames, games: fakeGames})

				let left = entries.length
				for(const { quest, appData } of entries) {
					const { taskData } = taskOf(quest)
					const missing = taskData.target - progressOf(quest, "PLAY_ON_DESKTOP")
					console.log(`Spoofed your game to ${appData.name}. Wait for ${Math.ceil(missing / 60)} more minutes.`)

					trackProgress(quest, "PLAY_ON_DESKTOP", taskData.target, () => {
						if(--left === 0) {
							RunningGameStore.getRunningGames = realGetRunningGames
							RunningGameStore.getGameForPID = realGetGameForPID
							FluxDispatcher.dispatch({type: "RUNNING_GAMES_CHANGE", removed: fakeGames, added: [], games: []})
						}
					})
				}
			})
		}
	}

	const streamQuests = quests.filter(q => taskOf(q).taskName === "STREAM_ON_DESKTOP")
	if(streamQuests.length) {
		if(!isApp) {
			console.log("STREAM_ON_DESKTOP quests need the discord desktop app, skipping:", streamQuests.map(questName).join(", "))
			remaining -= streamQuests.length
		} else {
			Promise.all(streamQuests.map(async quest => {
				const { taskData } = taskOf(quest)
				const applicationId = quest.config.application?.id ?? taskData.applications?.[0]?.id
				return { quest, applicationId, pid: Math.floor(Math.random() * 30000) + 1000 }
			})).then(entries => {
				const realFunc = ApplicationStreamingStore.getStreamerActiveStreamMetadata
				let index = 0
				ApplicationStreamingStore.getStreamerActiveStreamMetadata = () => {
					const entry = entries[index++ % entries.length]
					return { id: entry.applicationId, pid: entry.pid, sourceName: null }
				}

				let left = entries.length
				for(const { quest } of entries) {
					const { taskData } = taskOf(quest)
					const missing = taskData.target - progressOf(quest, "STREAM_ON_DESKTOP")
					console.log(`Spoofed your stream to ${questName(quest)}. Stream any window in vc for ${Math.ceil(missing / 60)} more minutes.`)

					trackProgress(quest, "STREAM_ON_DESKTOP", taskData.target, () => {
						if(--left === 0) ApplicationStreamingStore.getStreamerActiveStreamMetadata = realFunc
					})
				}
				console.log("Remember that you need at least 1 other person to be in the vc!")
			})
		}
	}
}
