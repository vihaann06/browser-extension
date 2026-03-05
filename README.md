# Hypothesis Browser Extension — Tag-Based Highlight Colors

A modified [Hypothesis](https://hypothes.is) browser extension that colors
annotation highlights based on their tags. Uses the **standard Hypothesis API**
— no custom server required.

## Tag-to-Color Mapping

| Tag        | Color  | Aliases                |
| ---------- | ------ | ---------------------- |
| `yellow`   | Yellow | `key`, `highlight`     |
| `pink`     | Pink   | `review`               |
| `orange`   | Orange | `important`            |
| `green`    | Green  | `question`             |
| `purple`   | Purple | `note`                 |
| `grey`     | Grey   | `misc`                 |

Tags are case-insensitive. The first matching tag on an annotation determines
its color. Annotations without a color tag use the default Hypothesis styling.

## Quick Start

### Prerequisites

- [Node.js](https://nodejs.org/) (v18+)
- [Yarn](https://yarnpkg.com/) (v3)
- Chrome or Chromium-based browser
- A free [hypothes.is](https://hypothes.is/signup) account

### Build

```bash
git clone --recursive https://github.com/vihaann06/browser-extension.git
cd browser-extension
yarn install
make build
```

If you forgot `--recursive`, initialize the submodule manually:

```bash
git submodule update --init
```

### Load in Chrome

1. Go to `chrome://extensions`
2. Enable **Developer mode** (top-right toggle)
3. Click **Load unpacked** and select the `build/` directory
4. **Disable the official Hypothesis extension** if you have it installed (they
   share the same extension ID for OAuth compatibility)

### Use

1. Navigate to any webpage
2. Click the Hypothesis icon or press the toolbar button to open the sidebar
3. Log in with your [hypothes.is](https://hypothes.is) account
4. Annotate text and add a color tag (e.g. `pink`, `orange`, `green`)
5. The highlight appears in the corresponding color

## How It Works

This fork adds tag-based coloring entirely on the client side. The modified
[client](https://github.com/vihaann06/client) is included as a git submodule
in the `client/` directory.

Key changes in the client:

- **`src/annotator/tag-highlight-color.ts`** — maps tags to CSS color classes
- **`src/annotator/guest.ts`** — applies tag color when highlighting
- **`src/sidebar/services/frame-sync.ts`** — passes `tags` to the guest frame
- **`src/styles/annotator/highlights.scss`** — CSS for tag-based colors

All annotation data is stored on and fetched from the public `hypothes.is` API.
No custom server is needed.

## Development

The extension wraps the Hypothesis client. The client submodule is automatically
built when you run `make build`. To rebuild after making changes to the client:

```bash
make build
```

Then reload the extension in `chrome://extensions`.

To build only the client:

```bash
make build-client
```

## License

Released under the [2-Clause BSD License](LICENSE).
