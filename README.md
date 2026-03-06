# Rive Framer Player

A Framer component to embed Rive animations with full scripting support, feathering, and all fit/alignment options.

Created by [Tom Acco](https://rivemasterclass.com), instructor of Rive Masterclass.

## Features

- Rive Scripting support
- Feathering & blur effects (WebGL2)
- All fit modes (contain, cover, fill, fitWidth, fitHeight, none, scaleDown)
- All alignment options
- Custom artboard & state machine selection
- Pointer interactions (click, hover)

## Quick Start

### 1. Add the Component to Framer

In your Framer project, create a new **Code Component** and paste this code:

```tsx
import { addPropertyControls, ControlType } from "framer"

export default function RivePlayer(props) {
    const { src, baseUrl, artboard, stateMachine, fit, alignment, debug } = props
    
    const playerUrl = "https://tomaccoux.github.io/rive-framer-player/player.html"
    
    let riveSrc = src
    if (src && !src.startsWith("http") && baseUrl) {
        riveSrc = baseUrl.replace(/\/$/, "") + "/" + src
    }
    
    if (!riveSrc || (!riveSrc.startsWith("http") && !baseUrl)) {
        return (
            <div style={{ 
                width: "100%", 
                height: "100%", 
                display: "flex", 
                alignItems: "center", 
                justifyContent: "center",
                fontFamily: "system-ui, sans-serif",
                fontSize: 14,
                color: "#666",
                padding: 20,
                textAlign: "center"
            }}>
                Enter a Rive file URL or set a Base URL + filename
            </div>
        )
    }
    
    const params = new URLSearchParams()
    params.set("src", riveSrc)
    if (artboard) params.set("artboard", artboard)
    if (stateMachine) params.set("sm", stateMachine)
    params.set("fit", fit || "contain")
    params.set("align", alignment || "center")
    
    const fullUrl = `${playerUrl}?${params.toString()}`

    if (debug) {
        return (
            <div style={{ padding: 10, fontSize: 12, wordBreak: "break-all", fontFamily: "monospace" }}>
                <strong>URL:</strong> {fullUrl}
            </div>
        )
    }

    return (
        <iframe
            src={fullUrl}
            style={{
                width: "100%",
                height: "100%",
                border: "none",
                display: "block",
            }}
            allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope"
        />
    )
}

addPropertyControls(RivePlayer, {
    debug: {
        type: ControlType.Boolean,
        title: "Debug",
        defaultValue: false,
    },
    baseUrl: {
        type: ControlType.String,
        title: "Base URL",
        defaultValue: "",
        placeholder: "https://raw.githubusercontent.com/you/repo/main/",
        description: "Base URL for your Rive files (optional if using full URLs)",
    },
    src: {
        type: ControlType.String,
        title: "Rive File",
        defaultValue: "",
        placeholder: "animation.riv or https://...",
        description: "Filename (requires Base URL) or full URL",
    },
    artboard: {
        type: ControlType.String,
        title: "Artboard",
        defaultValue: "",
        placeholder: "Default artboard",
    },
    stateMachine: {
        type: ControlType.String,
        title: "State Machine",
        defaultValue: "State Machine 1",
    },
    fit: {
        type: ControlType.Enum,
        title: "Fit",
        options: ["contain", "cover", "fill", "fitWidth", "fitHeight", "none", "scaleDown"],
        optionTitles: ["Contain", "Cover", "Fill", "Fit Width", "Fit Height", "None", "Scale Down"],
        defaultValue: "contain",
    },
    alignment: {
        type: ControlType.Enum,
        title: "Alignment",
        options: ["center", "topLeft", "topCenter", "topRight", "centerLeft", "centerRight", "bottomLeft", "bottomCenter", "bottomRight"],
        optionTitles: ["Center", "Top Left", "Top Center", "Top Right", "Center Left", "Center Right", "Bottom Left", "Bottom Center", "Bottom Right"],
        defaultValue: "center",
    },
})
```

### 2. Configure Your Rive Files

**Option A: Use Full URLs**
- Set `src` to the full URL of your Rive file
- Example: `https://your-cdn.com/animations/hero.riv`

**Option B: Use Base URL + Filename**
1. Host your `.riv` files on GitHub, CDN, or any static hosting
2. Set `Base URL` to your folder URL (e.g., `https://raw.githubusercontent.com/YOUR_USER/YOUR_REPO/main/`)
3. Set `src` to just the filename (e.g., `hero.riv`)

## Hosting Your Rive Files

### GitHub (Free)
1. Create a repository
2. Upload your `.riv` files
3. Use the raw URL: `https://raw.githubusercontent.com/USERNAME/REPO/main/`

### Other Options
- Cloudflare R2
- AWS S3
- Any static file hosting

## URL Parameters (Advanced)

You can also use the player directly via URL:

```
https://tomaccoux.github.io/rive-framer-player/player.html?src=YOUR_RIVE_URL&fit=cover&align=center
```

| Parameter | Example | Description |
|-----------|---------|-------------|
| `src` | `https://...file.riv` | Rive file URL (required) |
| `fit` | `cover` | contain, cover, fill, fitWidth, fitHeight, none, scaleDown |
| `align` | `topLeft` | center, topLeft, topCenter, topRight, centerLeft, centerRight, bottomLeft, bottomCenter, bottomRight |
| `artboard` | `MyArtboard` | Artboard name |
| `sm` | `State Machine 1` | State machine name |

## License

MIT
