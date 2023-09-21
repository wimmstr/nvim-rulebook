<!-- LTeX: enabled=false -->
# nvim-rulebook <!-- LTeX: enabled=true -->
<!-- TODO uncomment shields when available in dotfyle.com -->
<!-- <a href="https://dotfyle.com/plugins/chrisgrieser/nvim-rulebook"><img src="https://dotfyle.com/plugins/chrisgrieser/nvim-rulebook/shield" /></a> -->

Lookup rule documentation online. Add inline-comments that ignore diagnostic rules, like `// eslint-disable-line [ruleName]`.

Some LSPs provide code actions for to do that – this plugin adds commands for linters and LSPs that do not. 

---

<!--toc:start-->
- [Supported Sources](#supported-sources)
	- [Adding ignore comments](#adding-ignore-comments)
	- [Looking up rule documentation](#looking-up-rule-documentation)
- [Installation](#installation)
- [Configuration](#configuration)
- [Limitations](#limitations)
- [Credits](#credits)
<!--toc:end-->

## Supported Sources
You easily add a custom source via the [plugin configuration](#configuration). Though, please consider making a PR to add support for a linter if it is missing.

[Rule Data for the supported linters](./lua/rulebook/rule-data.lua)

### Adding ignore comments
<!-- TODO: auto-generate this list -->
- selene
- shellcheck
- vale
- LTeX
- yamllint
- stylelint
- typescript
- eslint
- biome
- pylint
- Pyright

### Looking up rule documentation
<!-- TODO: auto-generate this list -->
- selene 
- shellcheck 
- biome 
- yamllint 
- stylelint 
- eslint 
- Lua Diagnostics
- pylint 
- fallback 

## Installation
This plugin requires diagnostics provided by a source that supports neovim's builtin diagnostics system. (nvim's builtin LSP client or [nvim-lint](https://github.com/mfussenegger/nvim-lint) are such sources.)

```lua
-- lazy.nvim
{
	"chrisgrieser/nvim-rulebook",
	keys = {
		{ "<leader>i", function() require("rulebook").ignoreRule() end },
		{ "<leader>l", function() require("rulebook").lookupRule() end },
	}
},
```

```lua
-- packer
use { "chrisgrieser/nvim-rulebook" }

-- in your config
vim.keymap.set("n", "<leader>i", function() require("rulebook").ignoreRule() end)
vim.keymap.set("n", "<leader>l", function() require("rulebook").lookupRule() end)
```

## Configuration

```lua
defaultConfig = {
	ignoreRuleComments = {
		selene = {
			comment = "-- selene: allow(%s)",
			location = "prevLine",
		},
		-- ... (full list of supported sources can be found in the README)

		yourCustomSource = {
			-- %s will be replaced with rule-id
			-- if location is "encloseLine", needs to be a list of two strings
			comment = "// disabling-comment %s",

			-- "prevLine"|"sameLine"|"encloseLine"
			location = "prevLine",
		}
	},

	-- %s will be replaced with rule-id
	ruleDocumentations = {
		selene = "https://kampfkarren.github.io/selene/lints/%s.html"
		-- ... (full list of supported sources can be found in the README)

		yourCustomSource = "https://my-docs/%s.hthml"

		-- Search URL when no documentation definition is available for a
		-- diagnostic source. "%s" will be replaced with the diagnostic source & code.
		-- Default is the DDG "Ducky Search" (automatically opening first result).
		fallback = "https://duckduckgo.com/?q=%s+%21ducky&kl=en-us",
	}

	-- If no diagnostic is found, in current line, search this meany lines 
	-- forward for diagnostics before aborting.
	forwSearchLines = 10,
}
```

The plugin uses [vim.ui.select](https://neovim.io/doc/user/lua.html#vim.ui.select()) so the appearance of the rule selection can be customized by using a ui-plugin like [dressing.nvim](https://github.com/stevearc/dressing.nvim).

## Limitations
- The diagnostics have to contain the necessary data, [that is a diagnostic code and diagnostic source](https://neovim.io/doc/user/diagnostic.html#diagnostic-structure). Most LSPs and most linters configured for `nvim-lint` do that, but some diagnostic sources do not (for example `efm-langserver` with incorrectly defined errorformat). Please open an issue at the diagnostics provider to fix.
- This plugin does *not* hook into `vim.lsp.buf.code_action`, but provides its own selector.

## Credits
<!-- vale Google.FirstPerson = NO -->
__About Me__  
In my day job, I am a sociologist studying the social mechanisms underlying the digital economy. For my PhD project, I investigate the governance of the app economy and how software ecosystems manage the tension between innovation and compatibility. If you are interested in this subject, feel free to get in touch.

__Blog__  
I also occasionally blog about vim: [Nano Tips for Vim](https://nanotipsforvim.prose.sh)

__Profiles__  
- [reddit](https://www.reddit.com/user/pseudometapseudo)
- [Discord](https://discordapp.com/users/462774483044794368/)
- [Academic Website](https://chris-grieser.de/)
- [Twitter](https://twitter.com/pseudo_meta)
- [Mastodon](https://pkm.social/@pseudometa)
- [ResearchGate](https://www.researchgate.net/profile/Christopher-Grieser)
- [LinkedIn](https://www.linkedin.com/in/christopher-grieser-ba693b17a/)

__Buy Me a Coffee__  
<br>
<a href='https://ko-fi.com/Y8Y86SQ91' target='_blank'><img height='36' style='border:0px;height:36px;' src='https://cdn.ko-fi.com/cdn/kofi1.png?v=3' border='0' alt='Buy Me a Coffee at ko-fi.com' /></a>
