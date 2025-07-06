<script>
import { createEventDispatcher, onMount } from "svelte";
import { darkTheme } from "../../themeStore.js";

const dispatch = createEventDispatcher();
export let activeNav = 0;
let isDark;
let expanded = false;

const navOptions = [
    { icon: 'fi fi-rr-dashboard', label: 'Dashboard', id: 0 },
    { icon: 'fi fi-rr-user-graduate', label: 'Students', id: 1 },
    { icon: 'fi fi-rr-building', label: 'Hostel', id: 2 },
    { icon: 'fi fi-rr-hamburger-soda', label: 'Services', id: 3 },
    { icon: 'fi fi-rr-messages-question', label: 'Queries', id: 4 },
    { icon: 'fi fi-rr-shield-keyhole', label: 'Security', id: 5 },
];

function changeRoute(id) {
    activeNav = id;
    dispatch('changeRoute', id);
    if (window.innerWidth < 768) {
        expanded = false;
    }
}

function toggleSidebar() {
    expanded = !expanded;
}

darkTheme.subscribe(value => {
    isDark = value;
});

</script>

<aside class={expanded ? 'expanded' : ''} class:dark={isDark}>
    <div class="logo-container">
        <img src={isDark ? "./resources/logo-dark.png" : "./resources/logo-default.png"} alt="Persona" class="logo">
        {#if expanded}
            <span class="logo-text">Persona</span>
        {/if}
        <button class="toggle-btn" on:click={toggleSidebar}>
            <i class={expanded ? "fi fi-rr-angle-left" : "fi fi-rr-angle-right"}></i>
        </button>
    </div>
    
    <nav>
        {#each navOptions as { icon, id, label }}
            <button 
                class="nav-item" 
                class:active={activeNav === id} 
                on:click={() => changeRoute(id)}
                aria-label={label}
            >
                <i class={icon}></i>
                {#if expanded}
                    <span>{label}</span>
                {/if}
            </button>
        {/each}
    </nav>
    
    <div class="account" on:click={() => changeRoute(6)}>
        <div class="avatar">
            <img src="./resources/user-default.png" alt="User Account">
        </div>
        {#if expanded}
            <div class="user-info">
                <span class="user-name">Admin User</span>
                <span class="user-role">Administrator</span>
            </div>
        {/if}
    </div>
    
    <button class="theme-toggle" on:click={() => darkTheme.update(v => !v)}>
        <i class={isDark ? "fi fi-rr-sun" : "fi fi-rr-moon"}></i>
        {#if expanded}
            <span>{isDark ? "Light Mode" : "Dark Mode"}</span>
        {/if}
    </button>
</aside>

<style>
    aside {
        position: fixed;
        top: 0;
        left: 0;
        height: 100vh;
        background-color: white;
        width: 5rem;
        display: flex;
        flex-direction: column;
        box-shadow: var(--shadow-md);
        transition: width var(--transition-normal);
        z-index: 100;
        overflow-x: hidden;
    }
    
    aside.expanded {
        width: 16rem;
    }
    
    aside.dark {
        background-color: var(--card-dark);
        color: var(--text-light);
    }
    
    .logo-container {
        display: flex;
        align-items: center;
        padding: 1.5rem 1rem;
        position: relative;
    }
    
    .logo {
        width: 2.5rem;
        height: 2.5rem;
        object-fit: contain;
    }
    
    .logo-text {
        font-size: 1.25rem;
        font-weight: 600;
        margin-left: 1rem;
        white-space: nowrap;
    }
    
    .toggle-btn {
        position: absolute;
        right: 0.5rem;
        top: 50%;
        transform: translateY(-50%);
        background: transparent;
        color: var(--text-muted);
        padding: 0.5rem;
        border-radius: var(--radius-full);
        transition: all var(--transition-fast);
    }
    
    .toggle-btn:hover {
        background-color: rgba(0, 0, 0, 0.05);
        transform: translateY(-50%);
    }
    
    .dark .toggle-btn:hover {
        background-color: rgba(255, 255, 255, 0.1);
    }
    
    nav {
        flex: 1;
        display: flex;
        flex-direction: column;
        gap: 0.5rem;
        padding: 1rem 0.75rem;
        overflow-y: auto;
    }
    
    .nav-item {
        display: flex;
        align-items: center;
        padding: 0.75rem;
        border-radius: var(--radius-md);
        background: transparent;
        color: var(--text-muted);
        transition: all var(--transition-fast);
        cursor: pointer;
        width: 100%;
        justify-content: flex-start;
    }
    
    .nav-item i {
        font-size: 1.25rem;
        min-width: 1.5rem;
    }
    
    .nav-item span {
        margin-left: 1rem;
        white-space: nowrap;
        font-weight: 500;
    }
    
    .nav-item:hover {
        background-color: rgba(0, 0, 0, 0.05);
        color: var(--text-dark);
        transform: translateY(0);
    }
    
    .dark .nav-item:hover {
        background-color: rgba(255, 255, 255, 0.1);
        color: var(--text-light);
    }
    
    .nav-item.active {
        background-color: var(--primary-color);
        color: white;
    }
    
    .account {
        display: flex;
        align-items: center;
        padding: 1rem;
        border-top: 1px solid rgba(0, 0, 0, 0.1);
        cursor: pointer;
        transition: background-color var(--transition-fast);
    }
    
    .dark .account {
        border-top: 1px solid rgba(255, 255, 255, 0.1);
    }
    
    .account:hover {
        background-color: rgba(0, 0, 0, 0.05);
    }
    
    .dark .account:hover {
        background-color: rgba(255, 255, 255, 0.05);
    }
    
    .avatar {
        width: 2.5rem;
        height: 2.5rem;
        border-radius: var(--radius-full);
        overflow: hidden;
        background-color: #e2e8f0;
        display: flex;
        align-items: center;
        justify-content: center;
    }
    
    .avatar img {
        width: 100%;
        height: 100%;
        object-fit: cover;
    }
    
    .user-info {
        margin-left: 1rem;
        overflow: hidden;
    }
    
    .user-name {
        display: block;
        font-weight: 500;
        white-space: nowrap;
        text-overflow: ellipsis;
        overflow: hidden;
    }
    
    .user-role {
        display: block;
        font-size: 0.75rem;
        color: var(--text-muted);
        white-space: nowrap;
    }
    
    .theme-toggle {
        display: flex;
        align-items: center;
        padding: 0.75rem 1rem;
        margin: 0.5rem;
        border-radius: var(--radius-md);
        background: transparent;
        color: var(--text-muted);
        transition: all var(--transition-fast);
        cursor: pointer;
        justify-content: flex-start;
    }
    
    .theme-toggle i {
        font-size: 1.25rem;
        min-width: 1.5rem;
    }
    
    .theme-toggle span {
        margin-left: 1rem;
        white-space: nowrap;
    }
    
    .theme-toggle:hover {
        background-color: rgba(0, 0, 0, 0.05);
        color: var(--text-dark);
        transform: translateY(0);
    }
    
    .dark .theme-toggle:hover {
        background-color: rgba(255, 255, 255, 0.1);
        color: var(--text-light);
    }
    
    @media (max-width: 768px) {
        aside {
            width: 4rem;
        }
        
        aside.expanded {
            width: 16rem;
        }
        
        .logo {
            width: 2rem;
            height: 2rem;
        }
    }
</style>