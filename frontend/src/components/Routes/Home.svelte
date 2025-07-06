<script>
import Card from "../uicomponents/card.svelte";
import LineGraph from "../uicomponents/Graphs/lineGraph.svelte";
import Doughnut from "../uicomponents/Graphs/doughnut.svelte";
import BarGraph from "../uicomponents/Graphs/barGraph.svelte";
import { onMount } from "svelte";
import { darkTheme } from "../../themeStore";

let complaints = [];
let quote = "Brevity is the Wit of the Soul";
let author = "William Shakespeare";
let fetchError = false;
let loading = true;
let isDark = false;
let stats = [
    { title: "Total Students", value: 1245, icon: "fi fi-rr-user", trend: "+5.2%", color: "blue" },
    { title: "Hostel Occupancy", value: "87%", icon: "fi fi-rr-building", trend: "+2.1%", color: "green" },
    { title: "Active Queries", value: 24, icon: "fi fi-rr-interrogation", trend: "-12%", color: "purple" },
    { title: "Security Alerts", value: 3, icon: "fi fi-rr-shield-exclamation", trend: "-50%", color: "red" }
];

darkTheme.subscribe(value => {
    isDark = value;
});

onMount(() => {
    getComplaints();
    getQuote();
    setTimeout(() => loading = false, 500); // Simulate loading
});

async function getQuote() {
    try {
        const response = await fetch('http://localhost:5000/api/generateQuote');
        if (response.ok) {
            const data = await response.json();
            quote = data.quote;
            author = data.author;
        }
    } catch (error) {
        console.error("Error fetching quote:", error);
        fetchError = true;
    }
}

async function getComplaints() {
    try {
        const response = await fetch(`http://localhost:5000/complaints/getAllComplaints`);
        if (response.ok) {
            complaints = await response.json();
        }
    } catch (error) {
        console.error("Error fetching complaints:", error);
        fetchError = true;
    }
}

</script>

<div class="dashboard">
    <header class="dashboard-header">
        <div>
            <h1>Dashboard</h1>
            <p class="text-muted">Welcome back, Admin</p>
        </div>
        <div class="date-display">
            {new Date().toLocaleDateString('en-US', { weekday: 'long', year: 'numeric', month: 'long', day: 'numeric' })}
        </div>
    </header>

    {#if fetchError}
    <div class="alert alert-error">
        <i class="fi fi-rr-exclamation"></i>
        <p>Unable to connect to server. Please make sure the server is running at http://localhost:5000</p>
    </div>
    {/if}

    <section class="stats-grid">
        {#each stats as stat}
            <div class="stat-card" style="--accent-color: var(--{stat.color}-accent)">
                <div class="stat-icon" style="background-color: var(--{stat.color}-light)">
                    <i class={stat.icon}></i>
                </div>
                <div class="stat-content">
                    <h3>{stat.title}</h3>
                    <div class="stat-value-container">
                        <span class="stat-value">{stat.value}</span>
                        <span class="stat-trend">{stat.trend}</span>
                    </div>
                </div>
            </div>
        {/each}
    </section>

    <div class="dashboard-grid">
        <div class="chart-card">
            <div class="card-header">
                <h2>Student Distribution</h2>
                <div class="card-actions">
                    <button class="icon-btn"><i class="fi fi-rr-settings-sliders"></i></button>
                    <button class="icon-btn"><i class="fi fi-rr-download"></i></button>
                </div>
            </div>
            <div class="chart-container">
                <Doughnut />
            </div>
        </div>

        <div class="chart-card">
            <div class="card-header">
                <h2>Monthly Activity</h2>
                <div class="card-actions">
                    <button class="icon-btn"><i class="fi fi-rr-settings-sliders"></i></button>
                    <button class="icon-btn"><i class="fi fi-rr-download"></i></button>
                </div>
            </div>
            <div class="chart-container">
                <BarGraph />
            </div>
        </div>

        <div class="chart-card">
            <div class="card-header">
                <h2>Yearly Trends</h2>
                <div class="card-actions">
                    <button class="icon-btn"><i class="fi fi-rr-settings-sliders"></i></button>
                    <button class="icon-btn"><i class="fi fi-rr-download"></i></button>
                </div>
            </div>
            <div class="chart-container">
                <LineGraph />
            </div>
        </div>

        <div class="complaints-card">
            <div class="card-header">
                <h2>Recent Complaints</h2>
                <button class="view-all-btn">View All</button>
            </div>
            
            <div class="complaints-list">
                {#if loading}
                    <div class="loading-spinner"></div>
                {:else if complaints.length > 0}
                    {#each complaints.slice(0, 4) as complaint}
                        <div class="complaint-item">
                            <div class="complaint-icon">
                                <i class="fi fi-rr-comment-exclamation"></i>
                            </div>
                            <div class="complaint-content">
                                <h4>{complaint.title}</h4>
                                <p>{complaint.description.length > 80 ? complaint.description.substring(0, 80) + '...' : complaint.description}</p>
                            </div>
                            <div class="complaint-actions">
                                <button class="icon-btn"><i class="fi fi-rr-check"></i></button>
                                <button class="icon-btn"><i class="fi fi-rr-trash"></i></button>
                            </div>
                        </div>
                    {/each}
                {:else}
                    <div class="empty-state">
                        <i class="fi fi-rr-check-circle"></i>
                        <p>No complaints at the moment</p>
                    </div>
                {/if}
            </div>
        </div>

        <div class="quote-card">
            <i class="fi fi-rr-quote-right quote-icon"></i>
            <blockquote>
                {quote}
            </blockquote>
            <cite>— {author}</cite>
        </div>
    </div>
</div>

<style>
    .dashboard {
        width: 100%;
        max-width: 1600px;
        margin: 0 auto;
    }

    .dashboard-header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 2rem;
    }

    .dashboard-header h1 {
        margin: 0;
        font-size: 1.75rem;
    }

    .date-display {
        font-size: 0.9rem;
        color: var(--text-muted);
        background-color: var(--card-light);
        padding: 0.5rem 1rem;
        border-radius: var(--radius-md);
        box-shadow: var(--shadow-sm);
    }

    :global(body.dark-theme) .date-display {
        background-color: var(--card-dark);
    }

    .alert {
        display: flex;
        align-items: center;
        gap: 0.75rem;
        padding: 1rem;
        border-radius: var(--radius-md);
        margin-bottom: 1.5rem;
    }

    .alert-error {
        background-color: rgba(249, 65, 68, 0.1);
        color: var(--error-color);
        border-left: 4px solid var(--error-color);
    }

    .alert i {
        font-size: 1.25rem;
    }

    .stats-grid {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(240px, 1fr));
        gap: 1.5rem;
        margin-bottom: 1.5rem;
    }

    .stat-card {
        background-color: var(--card-light);
        border-radius: var(--radius-lg);
        padding: 1.5rem;
        display: flex;
        align-items: center;
        gap: 1rem;
        box-shadow: var(--shadow-sm);
        border-left: 4px solid var(--accent-color);
        transition: transform var(--transition-fast);
    }

    :global(body.dark-theme) .stat-card {
        background-color: var(--card-dark);
    }

    .stat-card:hover {
        transform: translateY(-3px);
        box-shadow: var(--shadow-md);
    }

    .stat-icon {
        width: 3rem;
        height: 3rem;
        border-radius: var(--radius-md);
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .stat-icon i {
        font-size: 1.5rem;
        color: var(--accent-color);
    }

    .stat-content {
        flex: 1;
    }

    .stat-content h3 {
        margin: 0 0 0.25rem 0;
        font-size: 0.875rem;
        color: var(--text-muted);
        font-weight: 500;
    }

    .stat-value-container {
        display: flex;
        align-items: baseline;
        gap: 0.5rem;
    }

    .stat-value {
        font-size: 1.5rem;
        font-weight: 700;
    }

    .stat-trend {
        font-size: 0.75rem;
        color: var(--success-color);
    }

    /* Define accent colors */
    :root {
        --blue-accent: var(--primary-color);
        --blue-light: rgba(67, 97, 238, 0.15);
        --green-accent: #10b981;
        --green-light: rgba(16, 185, 129, 0.15);
        --purple-accent: #8b5cf6;
        --purple-light: rgba(139, 92, 246, 0.15);
        --red-accent: var(--error-color);
        --red-light: rgba(249, 65, 68, 0.15);
    }

    .dashboard-grid {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 1.5rem;
    }

    .chart-card {
        background-color: var(--card-light);
        border-radius: var(--radius-lg);
        padding: 1.5rem;
        box-shadow: var(--shadow-sm);
        display: flex;
        flex-direction: column;
    }

    :global(body.dark-theme) .chart-card {
        background-color: var(--card-dark);
    }

    .chart-card:nth-child(1) {
        grid-column: span 1;
        grid-row: span 2;
    }

    .chart-card:nth-child(2) {
        grid-column: span 2;
        grid-row: span 1;
    }

    .chart-card:nth-child(3) {
        grid-column: span 2;
        grid-row: span 1;
    }

    .complaints-card {
        grid-column: span 2;
        background-color: var(--card-light);
        border-radius: var(--radius-lg);
        padding: 1.5rem;
        box-shadow: var(--shadow-sm);
    }

    :global(body.dark-theme) .complaints-card {
        background-color: var(--card-dark);
    }

    .quote-card {
        grid-column: span 1;
        background-color: var(--primary-light);
        color: white;
        border-radius: var(--radius-lg);
        padding: 2rem;
        position: relative;
        display: flex;
        flex-direction: column;
        justify-content: center;
    }

    .quote-icon {
        position: absolute;
        top: 1rem;
        right: 1rem;
        font-size: 2rem;
        opacity: 0.2;
    }

    blockquote {
        margin: 0 0 1rem 0;
        font-size: 1.25rem;
        line-height: 1.6;
        font-style: italic;
    }

    cite {
        align-self: flex-end;
        font-style: normal;
        font-weight: 500;
    }

    .card-header {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-bottom: 1rem;
    }

    .card-header h2 {
        margin: 0;
        font-size: 1.25rem;
    }

    .card-actions {
        display: flex;
        gap: 0.5rem;
    }

    .icon-btn {
        background: transparent;
        color: var(--text-muted);
        width: 2rem;
        height: 2rem;
        padding: 0;
        border-radius: var(--radius-full);
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .icon-btn:hover {
        background-color: rgba(0, 0, 0, 0.05);
        color: var(--text-dark);
        transform: translateY(0);
    }

    :global(body.dark-theme) .icon-btn:hover {
        background-color: rgba(255, 255, 255, 0.1);
        color: var(--text-light);
    }

    .chart-container {
        flex: 1;
        display: flex;
        align-items: center;
        justify-content: center;
        min-height: 200px;
    }

    .view-all-btn {
        background: transparent;
        color: var(--primary-color);
        font-size: 0.875rem;
        padding: 0.5rem 0.75rem;
    }

    .view-all-btn:hover {
        background-color: rgba(67, 97, 238, 0.1);
        transform: translateY(0);
    }

    .complaints-list {
        display: flex;
        flex-direction: column;
        gap: 1rem;
        margin-top: 1rem;
    }

    .complaint-item {
        display: flex;
        align-items: center;
        gap: 1rem;
        padding: 1rem;
        border-radius: var(--radius-md);
        background-color: var(--bg-light);
        transition: background-color var(--transition-fast);
    }

    :global(body.dark-theme) .complaint-item {
        background-color: var(--bg-dark);
    }

    .complaint-item:hover {
        background-color: rgba(0, 0, 0, 0.02);
    }

    :global(body.dark-theme) .complaint-item:hover {
        background-color: rgba(255, 255, 255, 0.05);
    }

    .complaint-icon {
        width: 2.5rem;
        height: 2.5rem;
        border-radius: var(--radius-full);
        background-color: rgba(249, 65, 68, 0.1);
        display: flex;
        align-items: center;
        justify-content: center;
    }

    .complaint-icon i {
        color: var(--error-color);
        font-size: 1.25rem;
    }

    .complaint-content {
        flex: 1;
    }

    .complaint-content h4 {
        margin: 0 0 0.25rem 0;
        font-size: 1rem;
    }

    .complaint-content p {
        margin: 0;
        font-size: 0.875rem;
        color: var(--text-muted);
    }

    .complaint-actions {
        display: flex;
        gap: 0.5rem;
    }

    .loading-spinner {
        width: 2rem;
        height: 2rem;
        border: 3px solid rgba(0, 0, 0, 0.1);
        border-top-color: var(--primary-color);
        border-radius: 50%;
        animation: spinner 1s ease-in-out infinite;
        margin: 2rem auto;
    }

    @keyframes spinner {
        to {
            transform: rotate(360deg);
        }
    }

    .empty-state {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        padding: 3rem 0;
        color: var(--text-muted);
    }

    .empty-state i {
        font-size: 3rem;
        margin-bottom: 1rem;
    }

    @media (max-width: 1200px) {
        .dashboard-grid {
            grid-template-columns: repeat(2, 1fr);
        }

        .chart-card:nth-child(1),
        .chart-card:nth-child(2),
        .chart-card:nth-child(3),
        .complaints-card,
        .quote-card {
            grid-column: span 2;
        }
    }

    @media (max-width: 768px) {
        .dashboard-grid {
            grid-template-columns: 1fr;
        }

        .stats-grid {
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
        }

        .chart-card:nth-child(1),
        .chart-card:nth-child(2),
        .chart-card:nth-child(3),
        .complaints-card,
        .quote-card {
            grid-column: span 1;
        }

        .dashboard-header {
            flex-direction: column;
            align-items: flex-start;
            gap: 1rem;
        }
    }
</style>