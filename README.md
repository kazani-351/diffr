# Diffr

A fast, private, and secure text comparison tool built with SvelteKit.

## Features

- **Real-time Diffing**: See differences between two text inputs instantly
- **Multiple Views**: Choose between unified and split view modes
- **Dark/Light Theme**: Automatic theme detection with manual toggle
- **Theme Persistence**: Your theme preference is saved locally
- **Responsive Design**: Works beautifully on desktop and mobile devices
- **Accessibility**: Keyboard navigation and ARIA labels for screen readers
- **Privacy**: All processing happens in your browser - no data is sent to any server

## Installation

```bash
npm install
```

## Development

```bash
npm run dev
```

Open [http://localhost:5173](http://localhost:5173) in your browser.

## Building

```bash
npm run build
```

## Preview Production Build

```bash
npm run preview
```

## Tech Stack

- [SvelteKit](https://kit.svelte.dev/) - The web framework
- [Svelte](https://svelte.dev/) - UI library
- [diff](https://www.npmjs.com/package/diff) - Text diffing algorithm
- [Vite](https://vitejs.dev/) - Build tool

## Usage

1. Paste your original text in the left panel
2. Paste your modified text in the right panel
3. See the differences appear in real-time
4. Toggle between unified and split views to compare in different formats
5. Use the "Load Example" button to see a demo
6. Toggle dark mode by clicking the moon/sun icon

## Browser Support

- Chrome (latest)
- Firefox (latest)
- Safari (latest)
- Edge (latest)

## Privacy

Diffr is a client-side only application. All text processing happens in your browser. No data is ever sent to any server, making it perfect for comparing sensitive code, documents, or any other text.

## License

MIT

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## Vibed with Shakespeare

Built with [Shakespeare](https://shakespeare.diy) - AI-powered web development.

[![Edit with Shakespeare](https://shakespeare.diy/badge.svg)](https://shakespeare.diy/clone?url=https%3A%2F%2Fgithub.com%2Fkazani-351%2Fdiffr.git)
