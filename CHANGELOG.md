# Changelog

All notable changes to this project will be documented in this file.

## [0.1.0] - 2026-01-08

### Added
- Initial release of Diffr
- Real-time text diffing between two text inputs
- Unified and split view modes
- Dark/light theme with automatic detection
- Theme persistence using localStorage
- Responsive design for mobile and desktop
- Accessibility features (ARIA labels, keyboard navigation)
- Error handling and loading states
- SEO files (robots.txt, sitemap.xml)
- Project documentation (README, CONTRIBUTING, LICENSE)
- GitHub and Shakespeare integration badges

### Improvements
- Enhanced scroll synchronization in split view
- Better performance with debouncing
- Improved stats calculation logic
- Enhanced CSS with better dark/light theme colors
- Added focus styles for better accessibility
- Added print styles for printing diffs
- Better error messages and user feedback

### Technical
- Fixed directory structure (removed nested src/src/)
- Created proper SvelteKit app.html template
- Added SVG favicon
- Added jsconfig.json for better IDE support
- Improved diff computation with try-catch error handling
- Added loading indicator during diff computation
