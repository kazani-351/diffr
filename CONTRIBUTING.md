# Contributing to Diffr

Thank you for your interest in contributing to Diffr! We welcome contributions from everyone.

## How to Contribute

### Reporting Bugs

If you find a bug, please open an issue on GitHub with:
- A clear description of the problem
- Steps to reproduce the issue
- Expected behavior vs. actual behavior
- Screenshots if applicable
- Your browser and version

### Suggesting Features

We love feature suggestions! Please:
- Check existing issues first to avoid duplicates
- Provide a clear description of the feature
- Explain why this feature would be useful
- Consider providing examples or mockups

### Submitting Pull Requests

1. **Fork the repository** and create your branch from `main`
2. **Make your changes** following the existing code style
3. **Test thoroughly** on different browsers and screen sizes
4. **Update documentation** if needed
5. **Commit your changes** with clear, descriptive messages
6. **Push to your fork** and submit a pull request

### Development Setup

```bash
# Clone your fork
git clone https://github.com/your-username/diffr.git
cd diffr

# Install dependencies
npm install

# Start development server
npm run dev
```

### Code Style

- Use meaningful variable and function names
- Add comments for complex logic
- Keep functions small and focused
- Follow the existing code formatting
- Ensure accessibility (ARIA labels, keyboard navigation)

### Testing

- Test on Chrome, Firefox, Safari, and Edge
- Test on mobile and desktop devices
- Test with dark and light themes
- Test with various text inputs (small, large, code, text)

### Commit Messages

Use clear, descriptive commit messages:
- `feat: add new feature`
- `fix: resolve issue with scroll sync`
- `docs: update README`
- `style: improve button hover states`
- `refactor: simplify diff calculation`
- `test: add unit tests for diff viewer`

## Guidelines

### Accessibility

- Always include ARIA labels for interactive elements
- Ensure keyboard navigation works
- Test with screen readers when possible
- Maintain sufficient color contrast

### Performance

- Use debouncing for expensive operations
- Avoid unnecessary re-renders
- Optimize for large text inputs
- Keep bundle size small

### Browser Compatibility

- Support modern browsers (Chrome, Firefox, Safari, Edge)
- Provide graceful degradation for older browsers
- Test across different devices

## Questions?

Feel free to open an issue if you have questions about contributing!

Thank you for contributing to Diffr! 🎉
