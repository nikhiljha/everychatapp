<script context="module">
	export async function preload({ params, query }) {
		// the `slug` parameter is available because
		// this file is called [slug].svelte
		const res = await this.fetch(`data/${params.slug}.json`);
		const data = await res.json();

		if (res.status === 200) {
			return { data };
		} else {
			this.error(res.status, data.message);
		}
	}

	function platformSupport(val) {
		switch(val) {
			case 0:
				return "Unsupported"
			case 1:
				return "Non-native (Electron) or Beta"
			case 2:
				return "Supported"
		}

		return "Error"
	}

	function yesno(val) {
		switch(val) {
			case true:
				return "Supported"
			case false:
				return "Unsupported"
		}

		return "Error"
	}

	function custom_preset_no(val) {
		switch(val) {
			case 0:
				return "Unsupported"
			case 1:
				return "Preset Only"
			case 2:
				return "Supported + Custom"
		}

		return "Error"
	}

	function e2ee(val) {
		switch(val) {
			case 0:
				return "Unsupported"
			case 1:
				return "Off by Default"
			case 2:
				return "Good"
		}

		return "Error"
	}

	function no_partial_full(val) {
		switch(val) {
			case 0:
				return "No"
			case 1:
				return "Partial"
			case 2:
				return "Full"
		}

		return "Error"
	}

	function group_dm_no(val) {
		switch(val) {
			case 0:
				return "Unsupported"
			case 1:
				return "1 on 1 Only"
			case 2:
				return "Supported in Groups and DMs"
		}

		return "Error"
	}

	function scale3(val) {
		switch (val) {
			case 2:
				return "✅"
			case 1:
				return "🤔"
			case 0:
				return "❌"
		}
	}
</script>

<script>
	export let data;
</script>

<style>
	/*
		By default, CSS is locally scoped to the component,
		and any unused styles are dead-code-eliminated.
		In this page, Svelte can't know which elements are
		going to appear inside the {{{post.html}}} block,
		so we have to use the :global(...) modifier to target
		all elements inside .content
	*/
	.content :global(h2) {
		font-size: 1.4em;
		font-weight: 500;
	}

	.content :global(pre) {
		background-color: #f9f9f9;
		box-shadow: inset 1px 1px 5px rgba(0,0,0,0.05);
		padding: 0.5em;
		border-radius: 2px;
		overflow-x: auto;
	}

	.content :global(pre) :global(code) {
		background-color: transparent;
		padding: 0;
	}

	.content :global(ul) {
		line-height: 1.5;
	}

	.content :global(li) {
		margin: 0 0 0.5em 0;
	}
</style>

<svelte:head>
	<title>{data.name} | Every Chat App</title>
</svelte:head>

<h1>{data.name}</h1>

<div class='content'>
	<!-- TODO: Make the details view prettier. -->
	<h2>Platform Support</h2>
	<ul>
		<li>Windows: { platformSupport(data.platform_support.windows) }</li>
		<li>MacOS: { platformSupport(data.platform_support.macos) }</li>
		<li>Linux: { platformSupport(data.platform_support.linux) }</li>
		<li>iOS: { platformSupport(data.platform_support.ios) }</li>
		<li>Android: { platformSupport(data.platform_support.android) }</li>
	</ul>

	<h2>Fun</h2>
	<ul>
		<li>Stickers: { custom_preset_no(data.fun.stickers) } </li>
		<li>GIFs: { yesno(data.fun.gif) } </li>
		<li>Games: { custom_preset_no(data.fun.games) } </li>
		<li>Emoji: { custom_preset_no(data.fun.emoji) } </li>
	</ul>

	<h2>Privacy</h2>
	<ul>
		<li>E2EE: { e2ee(data.privacy.e2ee) } </li>
		<li>OSS: { no_partial_full(data.privacy.oss) } </li>
		<li>Federated: { yesno(data.privacy.federated) } </li>
		<li>Decentralized: { yesno(data.privacy.decentralized) } </li>
		<li>Funding: { no_partial_full(data.privacy.funding_rating) } </li>
		<li>Funding Source: { data.privacy.funding_source || "Unknown" } </li>
	</ul>

	<h2>Customization</h2>
	<ul>
		<li>Themes: { custom_preset_no(data.customization.themes) }</li>
		<li>Plugins: { no_partial_full(data.customization.plugins) }</li>
	</ul>

	<h2>Chat Functions</h2>
	<ul>
		<li>Groups: { yesno(data.chat_functions.themes) }</li>
		<li>Maximum Group Size: { data.chat_functions.group_size || "N/A" }</li>
		<li>Mentions: { yesno(data.chat_functions.mentions) }</li>
		<li>@everyone: { yesno(data.chat_functions.ateveryone) }</li>
		<li>Voice Calls: { group_dm_no(data.chat_functions.voice) }</li>
		<li>Video Calls: { group_dm_no(data.chat_functions.video) }</li>
		<li>Message Deletion: { no_partial_full(data.chat_functions.deletion) }</li>
		<li>Voice Messages: { yesno(data.chat_functions.voice_messages) }</li>
		<li>Text Formatting: { no_partial_full(data.chat_functions.text_formatting) }</li>
	</ul>

	<h2>Media</h2>
	<ul>
		<li>Files: { custom_preset_no(data.media.files) }</li>
		<li>Max File Size: { data.media.file_size || "N/A" }</li>
		<li>File Limitations: { data.media.file_limitations || "Error" }</li>
		<li>Photos: { yesno(data.media.photo) }</li>
		<li>Videos: { yesno(data.media.video) }</li>
		<li>Location: { yesno(data.media.location) }</li>
		<li>Contact Sharing: { yesno(data.media.contact) }</li>
	</ul>
</div>
