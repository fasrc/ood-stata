# Open OnDemand Stata

<!-- Describe the app from a user's perspective. This is a simplied version of Overview -->
## FASRC users

Stata is an Open OnDemand app that launches Stata as an interactive session on a
compute node. It is designed for researchers who need to analyze, manage, and visualize data.

<!-- Link any relevant FASRC docs -->
### Using [Stata]
(https://docs.rc.fas.harvard.edu/kb/stata-on-cluster/)
<!-- Link how to create Sandbox -->
### Sandbox app

For how to create a Sandbox app, see the [Developing your own app using Open
OnDemand](https://docs.rc.fas.harvard.edu/kb/developing-apps-on-ood/)
documentation.

## Appverse overview

> [!NOTE]  
> This section is intended for sys-admins, developers, and power users.

Stata is an Open OnDemand Batch Connect app that launches Stata as an interactive session on HPC clusters. It is designed for researchers who need statistics, visualization, and data manipulation.

This app uses the Batch Connect `turbovnc` template with Slurm.

- **Upstream project:** [Stata](https://www.stata.com/)
- **Batch Connect template:** `turbovnc`
- **Scheduler:** Slurm

## Screenshots

<!-- A screenshot helps deployers verify their installation and helps users understand what they'll get. -->
<!-- Place images in a screenshots/ or docs/ directory. -->

Stata after re-organizing windows

![Stata running in browser](images/ZZZstata_desktop.png)

A [Stata plot example](ZZZhttps://documentation.stata.com/doc/en/pgmstatacdc/v_072/graphref/n1bwvjnj88q7dzn1014ai2v6a4jq.htm#n1bwvjnj88q7dzn1014ai2v6a4jq)

![Stata plot example](ZZZimages/stata_plot_example.png)

## Features

<!-- List the key capabilities specific to THIS OOD app (not the upstream software). -->ZZZ

- Launches Stata desktop GUI in a TurboVNC session with Xfce window manager
- Supports CPU and GPU execution
- Lmod module-based
- Configurable partition, memory, CPU cores, GPU cards, and wall time
- Optional additional Slurm options pass-through (long format)
- Optional reservation support for priority scheduling
- Optional email notification on job start
- OOM score management to prevent proxy errors on out-of-memory conditions

## Requirements

### Compute Node Software ZZZ

- Stata Lmod module and Stata license
- [Xfce Desktop](https://xfce.org/) 4+
- [Lmod](https://www.tacc.utexas.edu/research-development/tacc-projects/lmod)
  6.0.1+ or any other `module purge` and `module load <modules>` based CLI used
  to load appropriate environments within the batch job

### For VNC server support

- [TurboVNC](http://www.turbovnc.org/) 2.1+
- [websockify](https://github.com/novnc/websockify) 0.8.0+

### Open OnDemand

- Open OnDemand v3.0+
- [Slurm](https://slurm.schedmd.com/) job scheduler
- [Lmod](https://lmod.readthedocs.io/en/latest/) 

## App Installation

Please see the [References section](#software-installation) below for
instructions on how to install the software that is launched by this App.

### 1. Clone the repository

```bash
# Batch Connect apps:
cd /var/www/ood/apps/sys

git clone https://github.com/fasrc/ood-stata.git
cd ood-stata

# Pin to a release (recommended)
git checkout v1.0.0ZZZ
```

### 2. Configure for your site

<!-- Point deployers to the ONE place they need to edit. -->

<!-- Document ALL site-specific values and where they live. -->
<!-- This is the most important section for deployers at other sites. -->

<!-- Batch Connect apps: document form.yml attributes -->

#### `form.yml` attributes

Edit `form.yml` and update these values for your cluster:

| Attribute | Description | FASRC settings | Change to |
|-----------|-------------|---------| -----------|
| `cluster` | Target cluster ID | `odyssey` | Your cluster name |
| `bc_num_hours` | Maximum wall time (HH:MM:SS) | user-defined; default `04:00:00` | Your preferred default time |
| `bc_num_cores` | Number of cores | user-defined; default `1` | Your preferred default number of cores |
| `bc_queue` | Default scheduler partition | user-defined; default `shared` | Your preferred partition |
| `extra_slurm` | Extra slurm option (long-format) | user-defined | Remove if using aother scheduler |
| `custom_num_gpus` | Number of GPUs | user-defined; default `0` | Your preferred default number of GPUs |
| `memory` | Memory per job (GB) | user-defined; default: `4` | Your preferredmemory allocation |
| `stata_version` | Stata module to load on compute node | `stata/9.4-fasrc01` | Your `stata` module |ZZZ

#### `manifest.yml` attributes

Edit `manifest.yml` and update these values for your organization:

| Attribute | Change to |
|-----------|-----------|
| `description` | Your cluster and your documentation |

### 3. Verify

<!-- Batch Connect: -->
No OOD restart is needed (Batch Connect apps are detected automatically). Visit
your OOD dashboard and look for **Stata** under **Interactive Apps > Desktop
Apps**.


## Troubleshooting

### Job starts but app doesn't appear (Batch Connect)

1. Check the job's `output.log` in `~/ondemand/data/sys/YOUR-APP/`
2. Verify the module loads correctly: `module load software/1.0`
3. For VNC apps, verify the window manager is installed: `which xfwm4`

### "Module not found" error

The module name in `form.yml` doesn't match your system. Run `module spider
software` to find the correct name and update the `modules` attribute.

### Connection timeout

The app may need more time to start. Increase the connection timeout or check
that the compute node can open the required port.

## Testing

<!-- Where has this app been deployed and verified? -->
| Site | Operating System* | OOD Version | Scheduler | Status |
|------|------------------|-------------|-----------|--------|
| FASRC | Rocky 8.10 | 3.1 | Slurm 25.11 | Tested |
| FASRC | Rocky 8.10 | 4.0 | Slurm 25.11 | Tested |
| FASRC | Rocky 8.10 | 4.1 | Slurm 25.11 | Tested |

> [!NOTE]
> \*Operating system of compute nodes

<!-- How can a deployer verify it works? -->

To verify your installation:

1. Launch the app from the OOD dashboard with default settings
2. Confirm the application loads in the browser

## Known Limitations

<!-- Be honest about what doesn't work or hasn't been tested. -->

- Multi-node jobs are not supported
- Only tested on Centos 7 and Rocky 8; may not work on Ubuntu.

## Contributing

Contributions are welcome. To contribute:

1. [Fork this repository](https://github.com/fasrc/ood-stata/fork).
2. Create a feature branch (`git checkout -b feature/my-improvement`).
3. Submit a pull request with a description of your changes.

For bugs or feature requests, [open an issue](https://github.com/fasrc/ood-stata/issues).

This app is part of the [OOD Appverse](https://ondemand.connectci.org/affinity-groups/ood-appverse). Join the [Appverse Affinity Group](https://ondemand.connectci.org/affinity-groups/ood-appverse) to connect with other contributors.

## References

<!-- Credit upstream projects and any code you borrowed. -->

- [Stata](https://www.stata.com/en_us/home.html) — the application launched by this OOD app.
- [Open OnDemand](https://openondemand.org/) — the HPC portal framework.

### Software Installation

- [Stata Linux installation
  guide](https://support.stata.com/en/documentation/install-center/94/guide-for-unix.html).

## License

[MIT License](LICENSE).

## Acknowledgments

This work is supported by [FASRC](https://www.rc.fas.harvard.edu) at Harvard
Univesity.
