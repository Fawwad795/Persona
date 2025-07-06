<script>
import { onMount } from "svelte";
import { darkTheme } from "../../themeStore.js";

export let columnSpan = 1;
export let rowSpan = 1;
export let backgroundColor = null;
export let backgroundImage = null;
export let elevation = "md";
export let noPadding = false;
export let border = false;

let isDark = false;
let cardClass = "";

// Map elevation to shadow classes
const elevationMap = {
    "none": "",
    "sm": "shadow-sm",
    "md": "shadow-md",
    "lg": "shadow-lg"
};

darkTheme.subscribe(value => {
    isDark = value;
});

$: cardStyle = `
    grid-column: span ${columnSpan};
    grid-row: span ${rowSpan};
    ${backgroundColor ? `background-color: ${backgroundColor};` : ''}
    ${backgroundImage ? `background-image: url(${backgroundImage});` : ''}
    ${noPadding ? 'padding: 0;' : ''}
`;

$: cardClass = `card ${elevationMap[elevation] || 'shadow-md'} ${isDark ? 'dark' : ''} ${border ? 'with-border' : ''}`;

</script>

<div class={cardClass} style={cardStyle}>
    <slot></slot>
</div>

<style>
    .card {
        background-color: var(--card-light);
        border-radius: var(--radius-lg);
        padding: 1.5rem;
        height: 100%;
        transition: transform var(--transition-fast), box-shadow var(--transition-fast);
        display: flex;
        flex-direction: column;
        overflow: hidden;
    }
    
    .card.dark {
        background-color: var(--card-dark);
    }
    
    .shadow-sm {
        box-shadow: var(--shadow-sm);
    }
    
    .shadow-md {
        box-shadow: var(--shadow-md);
    }
    
    .shadow-lg {
        box-shadow: var(--shadow-lg);
    }
    
    .with-border {
        border: 1px solid rgba(0, 0, 0, 0.1);
    }
    
    .card.dark.with-border {
        border: 1px solid rgba(255, 255, 255, 0.1);
    }
    
    .card:hover {
        transform: translateY(-2px);
    }
    
    .shadow-sm:hover {
        box-shadow: var(--shadow-md);
    }
    
    .shadow-md:hover {
        box-shadow: var(--shadow-lg);
    }
</style>