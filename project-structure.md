# JSON Resume Elegant Theme - Project Structure

This document provides a comprehensive overview of the `jsonresume-theme-elegant` project structure, components, and workflow.

## Overview

The Elegant Theme is a responsive theme for [JSONResume](https://jsonresume.org/) that uses a card-based layout. It supports markdown in various resume sections and offers extensive social profile integration.

## Project Structure

```
jsonresume-theme-elegant/
├── .gitignore                # Git ignore file
├── Gruntfile.js              # Grunt task configuration
├── README.md                 # Project documentation
├── assets/                   # Theme assets
│   ├── css/                  # Compiled CSS
│   │   └── theme.css
│   └── less/                 # LESS source files
│       └── theme.less
├── build/                    # Build output directory
├── index.js                  # Main theme rendering logic
├── index.pug                 # Main Pug template
├── moment-precise-range.js   # Date difference calculation utility
├── package.json              # Project dependencies
├── pug/                      # Pug template components
│   ├── background/           # Background section templates
│   │   ├── about.pug
│   │   ├── awards.pug
│   │   ├── certificates.pug
│   │   ├── education.pug
│   │   ├── interests.pug
│   │   ├── projects-experience.pug
│   │   ├── publications.pug
│   │   ├── references.pug
│   │   ├── skills.pug
│   │   ├── volunteer-work.pug
│   │   └── work-experience.pug
│   ├── background-card.pug   # Background card wrapper
│   ├── floating-nav.pug      # Floating navigation component
│   ├── profile-card.pug      # Profile card component
│   ├── scripts.pug           # JavaScript scripts
│   └── stylesheets.pug       # CSS stylesheets
├── render.js                 # Resume rendering script
├── resume.json               # Example resume data
├── serve.js                  # Development server
└── tpl/                      # Compiled templates
    └── index.js              # Compiled Pug template
```

## Architecture Diagram

```mermaid
graph TD
    A[index.js] --> B[tpl/index.js]
    B --> C[Pug Templates]
    A --> D[moment-precise-range.js]
    A --> E[jsonresume-themeutils]
    A --> F[markdown-it]
    G[resume.json] --> A
    H[render.js] --> A
    I[serve.js] --> A
    J[Gruntfile.js] --> K[LESS Compilation]
    J --> L[Pug Compilation]
    J --> M[Server Start]
    K --> N[assets/css/theme.css]
    L --> O[tpl/index.js]
    C --> P[pug/profile-card.pug]
    C --> Q[pug/background-card.pug]
    C --> R[pug/floating-nav.pug]
    Q --> S[pug/background/*.pug]
```

## Data Flow

```mermaid
sequenceDiagram
    participant User
    participant Server as serve.js
    participant Renderer as render.js
    participant Theme as index.js
    participant Templates as tpl/index.js

    User->>Server: Access http://localhost:8888
    Server->>Renderer: Request resume render
    Renderer->>Theme: Provide resume.json data
    Theme->>Theme: Process resume data
    Theme->>Theme: Convert Markdown to HTML
    Theme->>Theme: Format dates
    Theme->>Templates: Pass processed data
    Templates->>Server: Return rendered HTML
    Server->>User: Display resume
```

## Component Relationships

```mermaid
flowchart TD
    subgraph Templates
        index.pug --> profile[profile-card.pug]
        index.pug --> bg[background-card.pug]
        index.pug --> nav[floating-nav.pug]
        index.pug --> scripts[scripts.pug]
        index.pug --> css[stylesheets.pug]

        bg --> about[background/about.pug]
        bg --> work[background/work-experience.pug]
        bg --> projects[background/projects-experience.pug]
        bg --> skills[background/skills.pug]
        bg --> education[background/education.pug]
        bg --> certs[background/certificates.pug]
        bg --> awards[background/awards.pug]
        bg --> volunteer[background/volunteer-work.pug]
        bg --> publications[background/publications.pug]
        bg --> interests[background/interests.pug]
        bg --> refs[background/references.pug]
    end

    subgraph Core
        renderer[index.js] --> md[markdown-it]
        renderer --> moment[moment-precise-range.js]
        renderer --> utils[jsonresume-themeutils]
        renderer --> templates[tpl/index.js]
    end

    subgraph Build
        grunt[Gruntfile.js] --> lessc[LESS Compile]
        grunt --> pugc[Pug Compile]
        grunt --> server[serve.js]
        lessc --> themeCSS[theme.css]
        pugc --> templates
    end

    resume[resume.json] --> Core
    Core --> html[Rendered HTML]
```

## Build Process

The theme uses Grunt for automating the build process:

1. **LESS Compilation**: Converts LESS files to CSS
2. **Pug Compilation**: Compiles Pug templates to JavaScript functions
3. **Build**: Processes resume data and renders the final HTML
4. **Server**: Runs a development server for previewing the resume

```mermaid
graph LR
    A[Development] --> B[Watch Files]
    B --> C{File Changed}
    C -->|LESS| D[Compile CSS]
    C -->|Pug| E[Compile Templates]
    D --> F[Reload Server]
    E --> F
    G[Build Command] --> H[Clean Directory]
    H --> I[Copy Assets]
    I --> J[Compile CSS]
    J --> K[Build Index]
    K --> L[Final Build]
```

## Key Components

### 1. Resume Data Processing

The `index.js` file contains the core logic for processing the resume data:

- Converting markdown to HTML
- Formatting dates
- Calculating durations
- Organizing skills with levels
- Structuring social profiles

### 2. Template Structure

The project uses Pug templates to generate HTML:

- **index.pug**: Main template structure
- **profile-card.pug**: Left sidebar with basic information
- **background-card.pug**: Main content area
- **floating-nav.pug**: Navigation menu

### 3. Responsive Design

The theme is fully responsive and includes:

- Floating navigation for desktop
- Collapsed navigation for mobile
- Card-based layout that adapts to screen size
- Bootstrap responsive grid

## Development Workflow

1. **Setup**:
   ```bash
   npm install -g grunt
   npm install -g pug-cli
   npm install
   ```

2. **Development**:
   ```bash
   grunt watch         # Watch for file changes
   grunt exec:run_server  # Run development server
   ```

3. **Build**:
   ```bash
   grunt build
   ```

## Project Extensions

The theme can be extended in several ways:

1. **Custom Styles**: Modify `assets/less/theme.less` to change the appearance
2. **New Sections**: Add new Pug templates in the `pug/background/` directory
3. **Additional Features**: Enhance `index.js` to process additional resume data
4. **Icon Support**: Add support for additional social profile icons

## Contributing Guidelines

When contributing to this project:

1. Create a feature branch from `master`
2. Follow the existing code style and naming conventions
3. Test changes locally using the development server
4. Submit a pull request with a clear description of changes