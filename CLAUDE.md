# CLAUDE.md - AI Assistant Guide for iPhone Wallpaper Generator

## Project Overview

**iPhone Wallpaper Generator** is a tool designed to create custom wallpapers optimized for iPhone devices. This document provides essential context for AI assistants working on this codebase.

### Project Status
- **Current State**: Fresh repository, development starting
- **Primary Goal**: Generate high-quality, customizable wallpapers for various iPhone models

## Repository Structure

### Recommended Project Layout

```
iphone-wallpaper-generator/
├── src/                    # Source code
│   ├── components/         # UI components (if web-based)
│   ├── generators/         # Wallpaper generation logic
│   ├── utils/             # Utility functions
│   ├── templates/         # Wallpaper templates/patterns
│   └── config/            # Configuration files
├── public/                # Static assets
├── tests/                 # Test files
├── docs/                  # Documentation
├── examples/              # Example wallpapers/usage
├── dist/                  # Build output (gitignored)
└── assets/                # Design assets, fonts, etc.
```

## iPhone Display Specifications (2024-2025)

### Current iPhone Models & Resolutions

| Model | Resolution | Aspect Ratio | PPI |
|-------|-----------|--------------|-----|
| iPhone 15 Pro Max / 14 Pro Max | 2796 × 1290 | 19.5:9 | 460 |
| iPhone 15 Pro / 14 Pro | 2556 × 1179 | 19.5:9 | 460 |
| iPhone 15 Plus / 14 Plus | 2796 × 1290 | 19.5:9 | 460 |
| iPhone 15 / 14 | 2532 × 1170 | 19.5:9 | 460 |
| iPhone SE (3rd gen) | 1334 × 750 | 16:9 | 326 |
| iPhone 13 Pro Max / 12 Pro Max | 2778 × 1284 | 19.5:9 | 458 |
| iPhone 13/13 Pro | 2532 × 1170 | 19.5:9 | 460 |

### Safe Zones
- **Status Bar**: ~150px from top
- **Home Indicator**: ~100px from bottom
- **Dynamic Island**: Consider notch/island area in wallpaper design

## Technology Stack Recommendations

### For Web-Based Generator

**Frontend:**
- React/Next.js or Vue.js for UI
- Canvas API or WebGL for rendering
- TailwindCSS or styled-components for styling
- TypeScript for type safety

**Backend (if needed):**
- Node.js/Express or Next.js API routes
- Sharp or Jimp for server-side image processing
- AWS S3 or similar for storage (optional)

### For CLI Tool

- Node.js with Commander.js or Yargs
- Sharp for image processing
- Inquirer.js for interactive prompts

### For Desktop App

- Electron with React
- Sharp or native Canvas for rendering

### Key Dependencies (Typical)
```json
{
  "sharp": "^0.32.0",           // Image processing
  "canvas": "^2.11.0",          // Drawing/rendering
  "color": "^4.2.0",            // Color manipulation
  "seedrandom": "^3.0.5",       // Deterministic randomness
  "commander": "^11.0.0"        // CLI (if applicable)
}
```

## Development Workflow

### Initial Setup
```bash
# Install dependencies
npm install

# Run development server/tool
npm run dev

# Build for production
npm run build

# Run tests
npm test

# Lint code
npm run lint
```

### Git Workflow
- **Main Branch**: `main` - production-ready code
- **Development**: Work on feature branches (`feature/wallpaper-type`, `fix/gradient-bug`)
- **Commits**: Use conventional commits (feat:, fix:, docs:, refactor:, test:)

## Key Features to Implement

### Core Features
1. **Gradient Generator**: Linear, radial, conic gradients
2. **Pattern Generator**: Geometric patterns, textures
3. **Image Overlays**: Add photos, blend modes
4. **Text/Typography**: Custom text overlays with various fonts
5. **Abstract Art**: Generative art algorithms
6. **Color Palette Management**: Preset and custom color schemes
7. **Export Options**: PNG, JPEG with quality control
8. **Batch Generation**: Create multiple variations

### Advanced Features
- AI-powered wallpaper generation (Stable Diffusion, DALL-E integration)
- Live preview with iPhone frame
- Calendar/weather integration
- Dynamic/animated wallpapers
- Perspective-aware designs (for parallax effect)

## Code Conventions

### File Naming
- Components: PascalCase (`WallpaperCanvas.tsx`)
- Utilities: camelCase (`generateGradient.ts`)
- Constants: UPPER_SNAKE_CASE (`COLOR_PALETTES.ts`)
- Tests: `*.test.ts` or `*.spec.ts`

### Code Style
- Use ESLint + Prettier for consistent formatting
- Prefer functional/declarative patterns
- Use TypeScript for type safety
- Document complex algorithms with comments
- Keep functions small and focused (single responsibility)

### Example Code Structure
```typescript
// generators/gradient.ts
export interface GradientConfig {
  type: 'linear' | 'radial' | 'conic';
  colors: string[];
  angle?: number;
  width: number;
  height: number;
}

export async function generateGradient(
  config: GradientConfig
): Promise<Buffer> {
  // Implementation
}
```

## Testing Strategy

### Test Types
- **Unit Tests**: Test individual generators/utilities
- **Integration Tests**: Test complete wallpaper generation pipeline
- **Visual Tests**: Compare generated images against snapshots
- **Performance Tests**: Ensure fast generation (<2s per wallpaper)

### Testing Tools
- Jest or Vitest for test runner
- Testing Library for component tests
- Pixelmatch for visual regression

## Performance Considerations

### Image Generation
- Use Sharp for performance (native C++ bindings)
- Implement caching for repeated operations
- Consider worker threads for batch processing
- Optimize asset loading (lazy load templates)

### Memory Management
- Stream large images when possible
- Clean up canvas/image buffers after use
- Limit concurrent generations

## AI Assistant Guidelines

### When Adding Features
1. **Read existing code first**: Understand current architecture
2. **Match existing patterns**: Follow established conventions
3. **Consider iPhone specs**: Ensure correct resolutions/aspect ratios
4. **Test visual output**: Verify generated wallpapers look correct
5. **Update documentation**: Keep this file and README current

### Common Tasks

#### Adding a New Wallpaper Generator
1. Create generator in `src/generators/`
2. Export interface and function
3. Add tests in `tests/generators/`
4. Update CLI/UI to expose new generator
5. Add example output to `examples/`

#### Modifying Resolutions
1. Update config in `src/config/resolutions.ts`
2. Test with various aspect ratios
3. Verify safe zones still work

#### Optimizing Performance
1. Profile with appropriate tools
2. Look for repeated computations
3. Consider caching/memoization
4. Test memory usage with batch operations

### Security Considerations
- Validate user inputs (colors, dimensions, file paths)
- Sanitize file names for exports
- Limit resource consumption (max image size, generation time)
- If web-based: Implement rate limiting, CSRF protection

### Accessibility (for web UI)
- Keyboard navigation support
- Screen reader compatibility
- Color contrast for UI elements
- Alt text for preview images

## Error Handling

### Common Errors
- Invalid dimensions (too large/small)
- Unsupported color formats
- Memory exhaustion
- File system permissions (for exports)

### Error Handling Pattern
```typescript
try {
  const wallpaper = await generateWallpaper(config);
  return { success: true, data: wallpaper };
} catch (error) {
  if (error instanceof ValidationError) {
    return { success: false, error: 'Invalid configuration' };
  }
  if (error instanceof MemoryError) {
    return { success: false, error: 'Image too large to process' };
  }
  throw error; // Re-throw unexpected errors
}
```

## Debugging Tips

### Visual Output Issues
- Save intermediate steps to debug pipeline
- Log color values and transformations
- Use visual diff tools for regression testing

### Performance Issues
- Profile with `console.time()` or profiling tools
- Check memory usage with `process.memoryUsage()`
- Test with various image sizes

### Common Pitfalls
- Color space conversions (RGB vs sRGB vs Display P3)
- Canvas coordinate systems (origin at top-left)
- Async/await in image processing chains
- Memory leaks from unclosed image handles

## Useful Resources

### Documentation
- [Sharp Documentation](https://sharp.pixelplumbing.com/)
- [Canvas API Reference](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API)
- [iPhone Display Specs](https://www.paintcodeapp.com/news/ultimate-guide-to-iphone-resolutions)

### Design Inspiration
- [Unsplash](https://unsplash.com/) - Free stock photos
- [Dribbble](https://dribbble.com/tags/wallpaper) - Design inspiration
- [iOS Wallpapers Reddit](https://www.reddit.com/r/iWallpaper/)

### Color Tools
- [Coolors](https://coolors.co/) - Color palette generator
- [Adobe Color](https://color.adobe.com/) - Color wheel and schemes

## Deployment

### Build Process
```bash
npm run build        # Build production bundle
npm run test         # Run all tests
npm run lint         # Check code quality
```

### Distribution Options
- **NPM Package**: For CLI tool
- **Web App**: Deploy to Vercel/Netlify
- **Desktop App**: Package with Electron Builder
- **Docker**: Containerize for server deployment

## Future Considerations

### Potential Enhancements
- Real-time collaboration on wallpaper design
- Mobile app (React Native)
- API for programmatic generation
- Plugin system for custom generators
- Integration with design tools (Figma, Sketch)
- Apple Watch face generation
- iPad/Mac wallpaper support

### Scalability
- CDN for generated wallpapers
- Queue system for batch jobs
- Distributed rendering for complex wallpapers
- Caching layer for popular presets

## Questions to Ask Before Starting Work

When a user requests work on this project, consider asking:

1. **Target Platform**: Web app, CLI tool, desktop app, or API?
2. **Primary Use Case**: Personal use, public service, or commercial?
3. **Tech Stack Preference**: React, Vue, vanilla JS, or other?
4. **Generation Approach**: Real-time in browser or server-side?
5. **AI Integration**: Should it support AI image generation?
6. **Deployment Target**: Where will this be hosted/run?

## Contact & Contribution

### Getting Help
- Check existing documentation
- Review example code
- Test with small inputs first

### Best Practices for AI Assistants
- Always validate iPhone resolution compatibility
- Test visual output, don't assume it works
- Consider performance for large images
- Keep user experience smooth and responsive
- Follow existing code patterns and conventions
- Update this document when architecture changes

---

**Last Updated**: 2026-01-13
**Status**: Initial project setup
**Next Steps**: Initialize project structure, choose tech stack, implement basic gradient generator
