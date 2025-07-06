<script>
import {onMount} from 'svelte';
import Header from './microcomponents/header.svelte';
import Home from "./Routes/Home.svelte";
import Student from './Routes/StudentRoute.svelte';
import Hostel from './Routes/HostelRoute.svelte';
import Services from './Routes/Services.svelte';
import Queries from './Routes/Queries.svelte';
import Security from './Routes/SecurityRoute.svelte';
import Account from './Routes/AccountRoute.svelte';
import { darkTheme } from '../themeStore';

let isDark;
let NavigationRoute = 0;
Header.activeNav = NavigationRoute;
const routes = {
    0: Home,
    1: Student,
    2: Hostel,
    3: Services,
    4: Queries,
    5: Security,
    6: Account
};

const HandleNavigation = (e) => {
    NavigationRoute = e.detail;
}

// Subscribe to the store
darkTheme.subscribe(value => {
    isDark = value;
    ChangeTheme();
});

function ChangeTheme() {
    const body = document.body;
    if (isDark) {  
        body.classList.add('dark-theme');
        body.classList.remove('light-theme');
    } else {
        body.classList.add('light-theme');
        body.classList.remove('dark-theme');
    }
}

onMount(() => {
   ChangeTheme();
});

</script>

<div class="dashboard-container" class:dark={isDark}>
    <Header on:changeRoute={HandleNavigation}/>
    <main class="content-area">
        <svelte:component this={routes[NavigationRoute]} />
    </main>
</div>

<style>
    .dashboard-container {
        display: flex;
        width: 100vw;
        height: 100vh;
        overflow: hidden;
        background-color: var(--bg-light);
    }
    
    .dashboard-container.dark {
        background-color: var(--bg-dark);
    }
    
    .content-area {
        flex: 1;
        margin-left: 5rem;
        padding: 1.5rem;
        overflow-y: auto;
        transition: margin-left var(--transition-normal);
        height: 100vh;
        box-sizing: border-box;
    }
    
    @media (min-width: 768px) {
        .content-area {
            padding: 2rem;
        }
    }
    
    /* When the sidebar is expanded */
    :global(aside.expanded) + .content-area {
        margin-left: 16rem;
    }
    
    @media (max-width: 768px) {
        .content-area {
            margin-left: 4rem;
            padding: 1rem;
        }
    }
</style>
