# Python1


# App.tsx #

### 1\. Initialize the Project

I recommend using **Vite** to quickly set up a React + TypeScript environment. Open your terminal and run:

```bash
# Create the project
npm create vite@latest banana-forecast -- --template react-ts

# Enter the directory
cd banana-forecast

# Install dependencies
npm install
```

### 2\. Install Required Packages

Your code relies on `recharts` for the graphs and `tailwindcss` for the styling.

```bash
# Install the charting library
npm install recharts

# Install Tailwind CSS and its peers
npm install -D tailwindcss postcss autoprefixer

# Initialize Tailwind configuration
npx tailwindcss init -p
```

### 3\. Configure Tailwind CSS (`tailwind.config.js`)

This is critical. Your code uses custom utility classes (like `bg-pop-yellow`, `shadow-hard`, `font-display`). You must update `tailwind.config.js` to define this custom theme:

```javascript
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./index.html",
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {
      // 1. Custom Fonts (Matches 'font-display' and 'font-body' in your code)
      fontFamily: {
        display: ['"Space Grotesk"', 'sans-serif'], 
        body: ['"Inter"', 'sans-serif'],
      },
      // 2. Custom Colors (The Pop Art Palette used in your code)
      colors: {
        'pop-yellow': '#F4D73B',
        'pop-magenta': '#E94E77',
        'pop-cyan': '#4AA3A2',
        'pop-pink': '#FF90B3',
        'pop-orange': '#FF6F3C',
        'pop-lime': '#C6D85B',
        'pop-purple': '#6A0572',
        'pop-green': '#009B77',
        'gray-100': '#F3F4F6', 
      },
      // 3. Custom Shadows (The "Hard" retro shadow effect)
      boxShadow: {
        'hard': '4px 4px 0px 0px #000000',
        'hard-sm': '2px 2px 0px 0px #000000',
        'hard-lg': '8px 8px 0px 0px #000000',
      }
    },
  },
  plugins: [],
}
```

### 4\. Global CSS Setup (`src/index.css`)

Update `src/index.css` to import Tailwind and the required fonts (Google Fonts) to match the aesthetic:

```css
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;700&family=Space+Grotesk:wght@700&display=swap');

@tailwind base;
@tailwind components;
@tailwind utilities;

body {
  /* Matches bg-pop-cyan/20 */
  background-color: #dcf0f2; 
}
```

### 5\. Essential File Structure

Your `App.tsx` imports many local components. To prevent build errors, you need to ensure the following directory structure exists (even if the files are just placeholders for now):

```text
banana-forecast/
├── public/
│   └── paopao.jpg           # Image referenced in the header
├── src/
│   ├── components/          # UI Components
│   │   ├── PopButton.tsx
│   │   ├── ChartFrame.tsx
│   │   ├── RetroTrendChart.tsx
│   │   ├── RetroWinRateChart.tsx
│   │   ├── RetroDistChart.tsx
│   │   ├── TacticalCalendar.tsx
│   │   └── StrategyGuide.tsx
│   ├── services/
│   │   └── geminiService.ts # API Service
│   ├── utils/
│   │   └── data.ts          # Data processing logic
│   ├── types/
│   │   └── index.ts         # TypeScript interfaces
│   ├── App.tsx              # Paste your code here
│   ├── main.tsx
│   └── index.css
├── package.json
└── tailwind.config.js
```

### 6\. Quick Fix for Missing Files (Mock Data)

Since you only provided `App.tsx`, the app will crash without the imported files. To get it running immediately, create these two minimal files:

**1. `src/types/index.ts`**

```typescript
export type CountryId = string;

export interface CountryData {
  id: CountryId;
  name: string;
  color: string;
  secondaryColor: string;
  data: Array<{
    year: number;
    price: number;
    type: 'history' | 'arima' | 'holt' | 'naive';
  }>;
}
```

**2. `src/utils/data.ts`**

```typescript
import { CountryData } from '../types';

export const MOCK_DATA: CountryData[] = [
  {
    id: 'columbia',
    name: 'Colombia',
    color: 'bg-pop-yellow',
    secondaryColor: '#F4D73B',
    data: [
      { year: 2020, price: 1.20, type: 'history' },
      { year: 2021, price: 1.25, type: 'history' },
      { year: 2026, price: 1.45, type: 'arima' }
    ]
  },
  // You can add more dummy data here...
];

export const transformJsonToCountryData = (json: any) => [];
export const getUnifiedChartData = () => [];
```

### 7\. Run the App

```bash
npm run dev
```

Open the link provided (usually `http://localhost:5173`), and you should see the Banana Forecast Dashboard.










# ChartFrame.tsx #


### 1\. Core Dependencies

To run this code, your project must have the following installed:

  * **React** (Functional components support)
  * **TypeScript** (For `interface` and type definitions)
  * **Tailwind CSS** (v3.0+)

### 2\. Required `tailwind.config.js`

The code uses custom utility classes that do not exist in default Tailwind. You **must** add the following extensions to your configuration file, or the styling will break (e.g., no shadows, wrong font).

```javascript
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {
      // 1. Custom Shadows (REQUIRED for 'shadow-hard-lg' & 'shadow-hard-sm')
      boxShadow: {
        'hard-lg': '8px 8px 0px 0px #000000',
        'hard-sm': '2px 2px 0px 0px #000000',
      },
      // 2. Custom Font (REQUIRED for 'font-display')
      fontFamily: {
        display: ['"Space Grotesk"', 'sans-serif'], 
      },
      // 3. Custom Colors (REQUIRED for 'hover:bg-pop-pink')
      colors: {
        'pop-pink': '#FF90B3',
      }
    },
  },
  plugins: [],
}
```

### 3\. Required Font Assets

The component uses `font-display`. You should import a bold, geometric font (like **Space Grotesk**) in your global CSS file to match the design intent.

**`src/index.css`**:

```css
@import url('https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@700&display=swap');

@tailwind base;
@tailwind components;
@tailwind utilities;
```

### 4\. File Placement

To ensure imports work correctly based on standard React patterns:

  * **File Path:** `src/components/ChartFrame.tsx`
  * **Usage:**
    ```tsx
    import { ChartFrame } from './components/ChartFrame';

    // Example Usage
    <ChartFrame title="My Chart" bgColor="bg-white">
       <p>Content goes here</p>
    </ChartFrame>
    ```

### Summary

Without the **`boxShadow`** and **`fontFamily`** settings in `tailwind.config.js`, the component will look flat and unstyled.













# RetroTrendChart.tsx #

### 1\. Dependencies

Ensure you have the charting library installed:

```bash
npm install recharts
```

### 2\. Google Fonts (Critical)

The code explicitly references three specific fonts in the Recharts props (`fontFamily`). If these are not loaded, the chart text will look like default Times New Roman.

Add this import to the top of your **`src/index.css`** file:

```css
/* src/index.css */
@import url('https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Inter:wght@400;700&family=Rubik+Mono+One&display=swap');

@tailwind base;
@tailwind components;
@tailwind utilities;
```

  * **Rubik Mono One**: Used for the Axis Ticks (The chunky, 8-bit looking numbers).
  * **Bebas Neue**: Used for the Legend (The tall, condensed uppercase text).
  * **Inter**: Used for standard labels.

### 3\. Tailwind Configuration (`tailwind.config.js`)

The `RPTooltip` sub-component inside the code uses a custom shadow class (`shadow-hard-sm`). You must define this in your config.

```javascript
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {
      // Required for className="shadow-hard-sm" inside the Custom Tooltip
      boxShadow: {
        'hard-sm': '2px 2px 0px 0px #000000',
      },
      // Optional: Ensure standard fonts map to the imported Google fonts if used elsewhere
      fontFamily: {
        mono: ['"Rubik Mono One"', 'monospace'], 
        sans: ['"Inter"', 'sans-serif'],
      }
    },
  },
  plugins: [],
}
```

### 4\. File Placement

Create the file in your components folder.

  * **File:** `src/components/RetroTrendChart.tsx`

### 5\. Usage Example

Now you can import and use it in your main App file. Note that this component has a fixed height container internally, so you should place it inside a container with defined dimensions.

```tsx
// src/App.tsx
import { RetroTrendChart } from './components/RetroTrendChart';

export default function App() {
  return (
    <div className="p-8 bg-gray-100 min-h-screen">
      <h1 className="text-3xl font-bold mb-6">Market Trends</h1>
      
      {/* The chart needs a container with height */}
      <div className="h-[500px] w-full border-4 border-black shadow-hard-sm">
        <RetroTrendChart />
      </div>
    </div>
  );
}
```

### Key Visual Features Explanation

  * **`type="stepAfter"`**: This Recharts prop is what gives the line that "staircase" or "robot" look, rather than a smooth curve.
  * **`PixelDot` component**: The custom SVG `<rect>` renders square points instead of circles, reinforcing the 8-bit style.
  * **Background Grid**: The code uses an inline `style` with `linear-gradient` to create the background grid graph paper effect. No external CSS is needed for this part.












# RetroWinRateChart.tsx #

### 1\. Core Dependencies

You need to install the Recharts library:

```bash
npm install recharts
```

### 2\. Key Font Configuration (Google Fonts)

The code uses inline styles to apply fonts to SVG elements (like Axes and Labels). If these fonts are not loaded in your HTML/CSS, Recharts will default to a basic serif font, breaking the design.

Add this import to the top of your **`src/index.css`** file:

```css
/* src/index.css */
@import url('https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Inter:wght@400;700&family=Rubik+Mono+One&display=swap');

@tailwind base;
@tailwind components;
@tailwind utilities;
```

  * **Bebas Neue**: Used for the Y-Axis country names (tall, condensed uppercase).
  * **Rubik Mono One**: Used for the percentage values at the end of the bars (retro, chunky 8-bit style).
  * **Inter**: Used for standard labels and descriptions.

### 3\. Tailwind CSS Configuration (`tailwind.config.js`)

The code relies on `shadow-hard-sm` and `font-display` / `font-mono` utility classes. Ensure your config file includes these extensions:

```javascript
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {
      // 1. Shadow Config (Required for CustomTooltip's className="shadow-hard-sm")
      boxShadow: {
        'hard-sm': '2px 2px 0px 0px #000000',
      },
      // 2. Font Config (Mapping classes used in the code to the imported fonts)
      fontFamily: {
        // "font-display" maps to Bebas Neue for headers
        display: ['"Bebas Neue"', 'sans-serif'], 
        // "font-mono" maps to the retro blocky font
        mono: ['"Rubik Mono One"', 'monospace'],
        // Default sans maps to Inter
        sans: ['"Inter"', 'sans-serif'],
      }
    },
  },
  plugins: [],
}
```

### 4\. File Placement

Save the code in your components directory:

  * **Path**: `src/components/RetroWinRateChart.tsx`

### 5\. Styling & Usage Details

  * **Background Dots**: The component uses an absolute `div` with a CSS `radial-gradient` to create a "halftone dot" background effect.
  * **Layout**: This is a `layout="vertical"` bar chart. It is responsive, meaning it expands to fill its parent. **You must place it inside a container with a defined height.**

**Usage Example**:

```tsx
// src/App.tsx
import { RetroWinRateChart } from './components/RetroWinRateChart';

export default function App() {
  return (
    <div className="p-8">
      <h2 className="text-2xl font-bold mb-4">Win Rate Analysis</h2>
      
      {/* Container MUST have height, or chart will be invisible */}
      <div className="h-[300px] w-full border-4 border-black p-4 shadow-hard-sm">
        <RetroWinRateChart />
      </div>
    </div>
  );
}
```










# TacticalCalendar.tsx #

### 1\. Core Dependencies

Ensure **Recharts** is installed:

```bash
npm install recharts
```

### 2\. Font Configuration

The chart relies on specific fonts to distinguish between the "Retro" data points and the standard UI. Ensure these are imported in `src/index.css`:

```css
/* src/index.css */
@import url('https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Inter:wght@400;900&family=Rubik+Mono+One&display=swap');

@tailwind base;
@tailwind components;
@tailwind utilities;
```

### 3\. Tailwind CSS Configuration (`tailwind.config.js`)

This component introduces new custom colors (`pop-purple`, `cyan`) that weren't used in previous components. **Update your config to include them**:

```javascript
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {
      colors: {
        // REQUIRED for this component
        'pop-purple': '#6A0572', 
        'cyan': '#00FFFF', // Used in border-cyan
        'gray-300': '#D1D5DB',
        'gray-500': '#6B7280',
      },
      boxShadow: {
        'hard': '4px 4px 0px 0px #000000',
        'hard-sm': '2px 2px 0px 0px #000000',
      },
      fontFamily: {
        display: ['"Bebas Neue"', 'sans-serif'], 
        body: ['"Inter"', 'sans-serif'],
        mono: ['"Rubik Mono One"', 'monospace'],
      }
    },
  },
  plugins: [],
}
```

### 4\. File Placement

Save the file in your components folder:

  * **Path:** `src/components/TacticalCalendar.tsx`

### 5\. Component Logic & Usage

This component is self-contained. It includes its own data constant (`SEASONALITY_DATA`) directly in the file, so it doesn't need external data props to render.

**Usage:**

```tsx
// src/App.tsx
import { TacticalCalendar } from './components/TacticalCalendar';

// It works immediately without props
<TacticalCalendar />
```

### Visual Features

  * **BestOptionDot**: The custom logic inside this component automatically calculates the lowest price for each month and draws a "Target" circle (black ring) around that data point on the chart.
  * **Pop-Purple Header**: The header uses a translucent purple background (`bg-pop-purple/20`), which requires the color definition in step 3 to work.














# RetroDistChart.tsx #

### 1\. Core Dependencies

Ensure **Recharts** is installed:

```bash
npm install recharts
```

### 2\. File Placement

This component depends on `ChartFrame` (which you set up earlier). Make sure the import path is correct.

  * **Path:** `src/components/RetroDistChart.tsx`
  * **Import Check:** The second line `import { ChartFrame } from './ChartFrame';` assumes `ChartFrame.tsx` is in the same folder. If `ChartFrame` is not used directly inside this file's return statement (it seems it isn't in the provided snippet, but it's imported), you can remove the import or wrap the whole return in `<ChartFrame>` if you intend to use it.

### 3\. Font Configuration

The chart uses three specific font families. Ensure they are imported in `src/index.css`:

```css
/* src/index.css */
@import url('https://fonts.googleapis.com/css2?family=Bebas+Neue&family=Inter:wght@400;700&family=Rubik+Mono+One&display=swap');

@tailwind base;
@tailwind components;
@tailwind utilities;
```

### 4\. Tailwind CSS Configuration (`tailwind.config.js`)

This component uses a wide range of custom colors (`pop-yellow`, etc.) and shadow utilities. Update your config to include:

```javascript
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {
      colors: {
        'pop-yellow': '#FACC15', // Matches Tailwind yellow-400 used in code
        'pop-magenta': '#E879F9',
        'pop-cyan': '#22D3EE',
        'pop-green': '#4ADE80',
        'gray-100': '#F3F4F6',
      },
      boxShadow: {
        'hard-sm': '2px 2px 0px 0px #000000',
      },
      fontFamily: {
        display: ['"Bebas Neue"', 'sans-serif'], 
        body: ['"Inter"', 'sans-serif'],
        mono: ['"Rubik Mono One"', 'monospace'],
      }
    },
  },
  plugins: [],
}
```

### 5\. Visual Explanation (Internal Logic)

  * **Scatter + Custom Shape**: The chart renders individual data points (using the `DotShape` component) to show the actual distribution of prices, creating a "swarm" effect.
  * **Custom Box Plot**: Instead of using Recharts' default box plot (which is limited), this code uses a `<Customized />` component (`RetroBoxPlotLayer`) to draw the box and whiskers manually using SVG lines and rects. This allows for the "Pop Art" black outlines.
  * **Strategy Cards**: The new section at the bottom (`StrategyCards`) uses standard Tailwind grid and flex classes but relies on the `border-4 border-black` and `shadow-hard-sm` utility classes to match the retro aesthetic.

### 6\. Usage Example

```tsx
// src/App.tsx
import { RetroDistChart } from './components/RetroDistChart';

export default function App() {
  return (
    <div className="p-8 max-w-4xl mx-auto">
      <h2 className="text-3xl font-bold mb-6">Price Distribution Analysis</h2>
      
      {/* Container needs height, but the chart component handles internal height well */}
      <div className="border-4 border-black bg-white p-4 shadow-hard">
        <RetroDistChart />
      </div>
    </div>
  );
}
```












# StrategyGuide.tsx #

### 1\. Core Dependencies

This component is lightweight and only requires **React**.

```bash
# If not already installed
npm install react react-dom
```

### 2\. Font Configuration

The component uses `font-display` for the headers (Comic style) and `font-body` for the speech bubbles. Import these in your global CSS.

**File:** `src/index.css`

```css
@import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;700&family=Space+Grotesk:wght@700&display=swap');

@tailwind base;
@tailwind components;
@tailwind utilities;
```

### 3\. Tailwind CSS Configuration (`tailwind.config.js`)

The component uses specific color names (`pop-yellow`, `pop-lime`, etc.) and shadow utilities (`shadow-hard`). You **must** add these to your config.

```javascript
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {
      // 1. Custom Colors (Matches class names: bg-pop-yellow, bg-pop-lime, etc.)
      colors: {
        'pop-yellow': '#F4D73B',
        'pop-cyan': '#22D3EE',
        'pop-magenta': '#E879F9',
        'pop-lime': '#C6D85B',  // Specifically used in Q4 card
        'gray-100': '#F3F4F6',
      },
      // 2. Custom Shadows (Matches class names: shadow-hard, shadow-hard-sm)
      boxShadow: {
        'hard': '4px 4px 0px 0px #000000',
        'hard-sm': '2px 2px 0px 0px #000000',
      },
      // 3. Custom Fonts
      fontFamily: {
        display: ['"Space Grotesk"', 'sans-serif'], // For the headers
        body: ['"Inter"', 'sans-serif'], // For the text
      }
    },
  },
  plugins: [],
}
```

### 4\. File Placement

Save the file in your components directory.

  * **Path:** `src/components/StrategyGuide.tsx`

### 5\. Usage Example

Since this component is self-contained (the data is hardcoded inside the `strategies` array), you can simply drop it into your main App.

```tsx
// src/App.tsx
import { StrategyGuide } from './components/StrategyGuide';

export default function App() {
  return (
    <div className="p-8 bg-gray-50 min-h-screen">
      <h1 className="text-4xl font-display font-bold mb-8 text-center">Market Analysis</h1>
      
      {/* The component handles its own layout grid */}
      <StrategyGuide />
      
    </div>
  );
}
```

### Visual Features Explanation

  * **Speech Bubble Effect**: The code uses a rotated div (`rotate-45`) with white background and black borders positioned absolutely (`absolute top-0 left-6`) to create the "tail" of the speech bubble coming from the header.
  * **Hover Effect**: The class `group hover:-translate-y-2` makes the card pop up slightly when the user hovers over it.
  * **Rotate Effect**: The section header uses `rotate-1` to give it a slightly askew, playful "Pop Art" feel.












# PopButton.tsx #
### 1\. Core Dependencies

  * **React** (Functional component)
  * **TypeScript** (Interface definitions)
  * **Tailwind CSS**

### 2\. Tailwind CSS Configuration (`tailwind.config.js`)

The component uses custom utility classes (`font-display`, `shadow-hard`, `bg-pop-cyan`). You must add these to your configuration file.

```javascript
/** @type {import('tailwindcss').Config} */
export default {
  content: [
    "./src/**/*.{js,ts,jsx,tsx}",
  ],
  theme: {
    extend: {
      // 1. Custom Font (Matches 'font-display')
      fontFamily: {
        display: ['"Space Grotesk"', 'sans-serif'], // Or any bold, geometric font
      },
      // 2. Custom Shadow (Matches 'shadow-hard')
      boxShadow: {
        'hard': '4px 4px 0px 0px #000000',
      },
      // 3. Custom Colors (Matches 'bg-pop-cyan' default prop)
      // Note: If you pass other colors like 'bg-pop-yellow' via props, define them here too.
      colors: {
        'pop-cyan': '#4AA3A2',
        'pop-yellow': '#F4D73B',
        'pop-magenta': '#E94E77',
        'gray-100': '#F3F4F6',
        'gray-400': '#9CA3AF',
      }
    },
  },
  plugins: [],
}
```

### 3\. Font Assets

Import the font used for `font-display` in your global CSS file.

**`src/index.css`**:

```css
@import url('https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@700&display=swap');

@tailwind base;
@tailwind components;
@tailwind utilities;
```

### 4\. File Placement

Save the file in your components directory.

  * **Path:** `src/components/PopButton.tsx`

### 5\. Usage Example

Here is how to use the button in a parent component (like `App.tsx`).

```tsx
import { useState } from 'react';
import { PopButton } from './components/PopButton';

export default function App() {
  const [isSelected, setIsSelected] = useState(false);

  return (
    <div className="p-10">
      <PopButton 
        label="Guatemala" 
        selected={isSelected} 
        onClick={() => setIsSelected(!isSelected)}
        // You can override the color using the prop
        colorClass="bg-pop-magenta" 
      />
    </div>
  );
}
```

### Visual Features

  * **External Texture:** The component uses a background image from `transparenttextures.com`. Ensure your application allows external image loading.
  * **State Logic:**
      * **Selected:** Bold border, custom color (`colorClass`), hard shadow, text moves slightly (`translate`) to simulate being pressed.
      * **Unselected:** Grayed out, no shadow, turns white/black on hover.
