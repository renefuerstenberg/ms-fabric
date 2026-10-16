# Microsoft Fabric Projects

Hands-on projects with **Microsoft Fabric** – from real-time data ingestion to dashboards,
alerts and AI agents. Each project lives in its own folder and is complete on its own:
code, KQL scripts, Fabric items and a detailed README.

## Projects

| Project | Fabric workloads | Description |
|---|---|---|
| [Power Mix Germany](real-time-intelligence-project-energy/) | Real-Time Intelligence: Eventstream, Eventhouse, Real-Time Dashboard, Activator, Data agent | Germany's power grid in real time: generation mix, renewable share, day-ahead prices, cross-border flows and grid frequency from the Energy-Charts API |

More projects will be added over time.

## Repository structure

```
ms-fabric/
├── README.md                      This overview
├── .gitignore                     Shared ignore rules for all projects
├── .vscode/                       Workspace settings (e.g. path of an Azure Function)
└── <project-folder>/              One folder per project
    ├── Readme.md                  Project documentation (architecture, setup, lessons learned)
    ├── <code>/                    e.g. Azure Functions for data ingestion
    ├── Fabric/                    KQL scripts and other files that are maintained by hand
    └── <Item>.<Type>/             Fabric items, written by Fabric Git integration
```

## How the Fabric items get here

Each project is connected to its own Fabric workspace through **Fabric Git integration**.
The workspace is connected to this repository with the project folder as **Git folder**, for
example:

| Setting | Value |
|---|---|
| Repository | `ms-fabric` |
| Branch | `main` |
| Git folder | `real-time-intelligence-project-energy` |

Fabric writes and updates the item folders (`*.Eventhouse`, `*.Eventstream`,
`*.KQLDashboard`, …) itself. Change them in Fabric and commit from there; everything else in
the project folder (code, KQL scripts, documentation) is maintained in this repository.

## Using a project

1. Open the project folder and read its `Readme.md` – it lists the prerequisites (Azure
   subscription, Fabric capacity, tenant settings) and the setup steps.
2. Create a Fabric workspace and connect it to this repository as described above, or import
   the item definitions manually.
3. Deploy the code (e.g. the Azure Function) and configure its settings.

Secrets such as connection strings are never stored in this repository. Local settings files
(`local.settings.json`) are excluded via `.gitignore`; use the `*.example` files as templates.

## Data sources and licenses

Each project names its data sources and their licenses in its own `Readme.md`. Please keep the
required attribution when you reuse data, screenshots or derived results.

## Author

**René Fürstenberg** – Microsoft Fabric, Azure analytics and Power BI

- Blog: [renefuerstenberg.de](https://renefuerstenberg.de)
- YouTube: [Cloud Junkies](https://www.youtube.com/@cloud-junkies)
- LinkedIn: [renefuerstenberg](https://www.linkedin.com/in/renefuerstenberg)