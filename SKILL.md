---
name: taltech-hpc-docs
description: "TalTech HPC Centre documentation reference — cluster, cloud, LUMI supercomputer. Every answer MUST cite the source doc page(s) the user can verify against."
version: 1.0.0
author: Hermes Agent (from docs.hpc.taltech.ee audit)
platforms: [linux, macos, windows]
metadata:
  hermes:
    tags: [taltech, hpc, cluster, slurm, lumi, cloud, ssh, billing, singularity]
    related_skills: []
---

# TalTech HPC Documentation Reference

Authoritative source: <https://docs.hpc.taltech.ee/>
Source repo: <https://github.com/TalTech-HPC-Centre/user-guides-source>

## RULE

After every answer, append the specific source page(s) the information came from in this format:

> **Source:** [Page Title](https://docs.hpc.taltech.ee/<page>.html)

If info spans multiple pages, list all of them. Do not answer from this skill alone — ALWAYS cite the source page so the user can verify and explore deeper.

---

## 1. Access & Prerequisites

### Requirements
- Active **Uni-ID account** (six letters from full name)
- Must be added to **HPC-USERS group** — email `hpcsupport@taltech.ee` from TalTech email with your UniID
- For licensed software: request addition to appropriate group
- **Linux command-line knowledge is essential** — [learning resources available](https://docs.hpc.taltech.ee/learning.html)
- Non-employees/non-students: [application form](https://taltech.atlassian.net/servicedesk/customer/portal/14/group/33/create/148)

**Source:** [Home — Access Requirements](https://docs.hpc.taltech.ee/)

### Access Methods
1. **Browser (OnDemand):** <https://ondemand.hpc.taltech.ee/> — graphical desktop session via web
2. **SSH:** `ssh -X -Y uni-ID@base.hpc.taltech.ee`
3. **VPN (off-campus):** [FortiVPN required](https://taltech.atlassian.net/wiki/spaces/ITI/pages/38994267/Kaug+hendus+FortiClient+VPN+Remote+connection+with+FortiClient+VPN)

**Source:** [Quickstart: Cluster — Accessing the cluster](https://docs.hpc.taltech.ee/quickstart.html)

### SSH Keys
- Server fingerprints for host verification:
  - `base.hpc.taltech.ee`: `ECDSA SHA256:OEfQiOB/eIG8hYoQ25sQk9T5tx9EtQbhi6sNM4C8mME`
  - `amp.hpc.taltech.ee`: `ECDSA SHA256:yl6+VaKow6qDZAXL3rQY8+3d3pcH0kYg7MjGgNVTWZs`
  - `viz.hpc.taltech.ee`: `ECDSA SHA256:z2/bxleZ3T3vErkg4C7kvDPKKEU0qaoR8bL29EgMfGA`
- Generate: `ssh-keygen -t ed25519` (Ed25519 recommended) or `-t rsa -b 4096`
- Upload: `ssh-copy-id Uni-ID@base.hpc.taltech.ee` (Linux/Mac) or PowerShell on Windows

**Source:** [SSH Keys](https://docs.hpc.taltech.ee/ssh.html)

---

## 2. Hardware Specifications

### TalTech Cluster (`base.hpc.taltech.ee`)

| Node type | Count | CPU | RAM | Storage/Notes |
|-----------|-------|-----|-----|---------------|
| **green** | 32 | 2× Xeon Gold 6148 (40C/80T per node) | 96 GB DDR4-2666 | 25 GbE, 18 with FDR IB (`green-ib`) |
| **ada** | 2 | 2× AMD EPYC 9354 Zen4 (64C/128T) | 1.5 TB | 2× Nvidia L40/48GB, AVX-512 |
| **amp** | 1 | 2× AMD EPYC 7742 Zen2 (128C/256T) | 1 TB | 8× Nvidia A100/40GB |
| **amp2** | 1 | 2× AMD EPYC 7713 Zen3 (128C/256T) | 2 TB | 8× Nvidia A100/80GB |
| **mem1tb** | 1 | 4× Xeon E5-4640 (32C/64T) | 1 TB | SandyBridge |
| **viz** | 1 | Xeon E5-2630L v2 (24T) | 64 GB | 2× Tesla K20Xm, 2 TB HDD |

- Storage: **1.5 PB** total, quotas: **500 GB** (HOME), **2 TB** (SMBHOME)
- HOME: `/gpfs/mariana/home/$USER/` — weekly backup (excludes cache, conda, venv)
- SMBHOME: `/gpfs/mariana/smbhome/$USER/` — mountable share, no backup
- SCRATCH: `/state/partition1/$JOBID/` — per-node, temporary, not shared between nodes

**Source:** [Home — Hardware Specification](https://docs.hpc.taltech.ee/) | [Hardware page](https://docs.hpc.taltech.ee/hardware.html) | [GPU page](https://docs.hpc.taltech.ee/gpu.html)

### ETAIS Cloud
- 5-node OpenStack cloud, 5 compute (nova) nodes with 768 GB RAM and 80 threads each
- 65 TB CephFS storage (net capacity)
- Access: <https://etais.ee/>

**Source:** [Home — Cloud](https://docs.hpc.taltech.ee/) | [Cloud Quickstart](https://docs.hpc.taltech.ee/cloud.html)

### LUMI Supercomputer
- HPE Cray EX, fastest in Europe
- **LUMI-G:** 2,560 nodes × 1 AMD Trento CPU + 4 AMD MI250X GPUs
- **LUMI-C:** 1,536 nodes × 2× 64-core AMD EPYC, 256-1024 GB RAM
- **LUMI-D:** Large memory nodes, 32 TB total for data analytics
- **LUMI-P:** 4× Lustre filesystems, 20 PB each, 240 GB/s bandwidth each
- **LUMI-F:** Flash storage, 8 PB, 1,740 GB/s bandwidth
- **LUMI-O:** Object store, 30 PB

**Source:** [LUMI Supercomputer](https://docs.hpc.taltech.ee/lumi.html)

---

## 3. SLURM Partitions

| Partition | Default time | Time limit | Default memory | Nodes |
|-----------|-------------|------------|----------------|-------|
| **short** | 10 min | 4 hours | 1 GB/thread | green, ada, amp |
| **common** | 10 min | 8 days | 1 GB/thread | green |
| **green-ib** | 10 min | 8 days | 1 GB/thread | green (InfiniBand) |
| **long** | 10 min | 22 days | 1 GB/thread | green |
| **gpu** | 10 min | 5 days | 1 GB/thread | amp, ada |
| **bigmem** | 10 min | 8 days | 1 GB/thread | ada, amp, mem1tb |

**Source:** [Home — SLURM Partitions](https://docs.hpc.taltech.ee/) | [Quickstart](https://docs.hpc.taltech.ee/quickstart.html)

### Node Features (for `--constraint=`)

| Feature | Description |
|---------|-------------|
| `A100-40` / `A100-80` / `L40` | GPU type |
| `nvcc80` / `nvcc89` / `nvcc35` | GPU compute capability |
| `zen2` / `zen3` / `zen4` | AMD CPU generation |
| `avx512` / `skylake` / `sandybridge` | CPU architecture |
| `ib` | InfiniBand network |

**Source:** [Quickstart — Node Features](https://docs.hpc.taltech.ee/quickstart.html)

---

## 4. Running Jobs with SLURM

### Common Commands
- `srun` — Start interactive session or app
- `sbatch` — Submit batch script
- `squeue` — Check job/queue status
- `sinfo` — Check partitions and nodes
- `scancel` — Delete/stop job
- `sacctmgr show associations format=account%30,user%30 | grep $USER` — List your SLURM accounts

### Account Prefixes
- `user_` — Personal account (small limit, ~10 EUR/quarter)
- `project_` — Project account
- `course_` — Course account
- Include `--account=SLURM-ACCOUNT` in every `srun` or `#SBATCH`

### Example Interactive Session
```bash
srun -t 01:00:00 --account=project_CHANGEME --pty bash
```

### Example Batch Script (`myjob.slurm`)
```bash
#!/bin/bash
#SBATCH --partition=common
#SBATCH --job-name=HelloOMP
#SBATCH --time=00:10:00
#SBATCH --nodes=4
#SBATCH --ntasks-per-node=7
#SBATCH --cpus-per-task=4
#SBATCH --account=project_CHANGEME
#SBATCH --mem-per-cpu=100
#SBATCH --array=13-18

export OMP_NUM_THREADS=$SLURM_CPUS_PER_TASK
module load gcc
./hello_omp $SLURM_ARRAY_TASK_ID
srun -n 28 ./hello_mpi
```

**Note:** OpenMP environment variables are NOT set automatically — you must export `OMP_NUM_THREADS` manually.

**Source:** [Quickstart — Running Jobs with SLURM](https://docs.hpc.taltech.ee/quickstart.html)

---

## 5. Module System (Lmod)

1. Load the SPACK module for free programs:
   ```bash
   module load rocky8-spack
   ```
2. Or load licensed/all programs:
   ```bash
   module load rocky8/all
   ```
3. List available: `module avail`
4. Load a program: `module load <name>`

**Module flags in `module avail` output:**
- **Lic** — License required
- **Uni** — Site licence, limited concurrent processes
- **Reg** — Registration required
- **L** — Module is loaded
- **Dp** — Deprecated
- **O** — Obsolete / superseded by SPACK
- **Exp** — Experimental
- **D** — Default module

**Source:** [Modules and Package Managers](https://docs.hpc.taltech.ee/modules.html) | [SPACK](https://docs.hpc.taltech.ee/spack.html)

---

## 6. Storage

| Path | Purpose | Quota | Backup |
|------|---------|-------|--------|
| `/gpfs/mariana/home/$USER/` | HOME directory | 500 GB | Weekly |
| `/gpfs/mariana/smbhome/$USER/` | SMB share (mountable) | 2 TB | None |
| `/gpfs/mariana/smbgroup/` | Group share (mountable) | No limit | None |
| `/state/partition1/$JOBID/` | Node-local scratch | Temporary | None |
| `/localstorage/` (amp1, amp2) | Fast NVMe (10 TB) | Long-lived | None |

**Source:** [Quickstart — Structure and file tree](https://docs.hpc.taltech.ee/quickstart.html) | [SMB Network Drive](https://docs.hpc.taltech.ee/samba.html)

---

## 7. Billing

### TalTech Cluster
| Resource | Unit | Standard | EU project |
|----------|------|----------|------------|
| CPUcore (< 6 GB RAM) | CPUcore·hour | €0.008 | €0.007 |
| CPUcore (> 6 GB RAM) | 6 GB RAM·hour | €0.008 | €0.007 |
| GPU | GPU·hour | €0.62 | €0.54 |
| Storage | 1 TB·hour | €0.007 | €0.006 |

### ETAIS Cloud
| Resource | Unit | Price |
|----------|------|-------|
| CPU | CPU·hour | €0.002 |
| Memory | RAM·hour | €0.001 |
| Storage | TB·hour | €0.03 (standard) / €0.025 (EU project) |

### LUMI (Estonian users)
| Resource | Unit | Price |
|----------|------|-------|
| CPUcore | CPUcore·hour | €0.008 |
| GPU | GPU·hour | €0.35 |
| User home | 20 GB | Free |
| Project storage | TB·hour | €0.0106 |
| Flash scratch | TB·hour | 10× standard |

### Administration
| Service | Price |
|---------|-------|
| Administrator service | €56/hour (standard), €49/hour (EU project) |

**Source:** [Billing page](https://docs.hpc.taltech.ee/billing.html) | [Home — Billing](https://docs.hpc.taltech.ee/)

---

## 8. Software

### Installation Methods
1. **Linux distribution packages** — free, recent enough, no modules needed
2. **SPACK** — free open-source via modules: `module load rocky8-spack`
3. **Manual installation** — mostly non-free: `module load rocky8/all`

Request missing software: email `hpcsupport@taltech.ee` or [Helpdesk Portal](https://helpdesk.taltech.ee/login.jsp)

### Categories (full list on docs site)
- **Engineering:** FreeCAD, Salome, Blender, Gmsh, Netgen, Cubit (affiliate), ElmerFEM, CalculiX, FreeFEM, deal.II, MFEM, code_aster, Abaqus, Comsol, LSDyna, Star-CCM+, OpenRadioss, OpenFOAM, SU2, WAM, SWAN
- **Chemistry/Biology:** Gaussian, ORCA, NWChem, XTB, TURBOMOLE, Multiwfn, VMD, etc.
- **Data Analysis:** Jupyter, Octave
- **Visualization:** ParaView, VisIt, Py-MayaVi, Molden, VMD, Ovito, Ospray, PoVray, GaussView, Avogadro

**Source:** [Software packages](https://docs.hpc.taltech.ee/software.html)

---

## 9. Containers (Singularity)

- Native Singularity CE 4.1.5 on Rocky 8 (no module to load)
- Pull Docker images: `singularity pull docker://ubuntu:18.04`
- Run via sbatch with `singularity exec`

**Source:** [Containers](https://docs.hpc.taltech.ee/singularity.html)

---

## 10. MPI

- OpenMPI installed via modules: `module load openmpi/4.1.1-gcc-10.3.0-r8-tcp`
- **Do NOT use `mpirun`** — use `srun`
- For MPI jobs: prefer `green-ib` partition or stay within single node (`-N 1`)
- Transport layers: TCP (default), openib (RDMA/verbs), UCX

**Source:** [MPI](https://docs.hpc.taltech.ee/mpi.html)

---

## 11. OnDemand (Browser-based HPC)

<https://ondemand.hpc.taltech.ee/> provides:
- Desktop session (xfce)
- File browser (upload/download/delete)
- Node load monitoring
- Job submission and cancellation
- Filesystem quota checks
- Billing dashboard
- Interactive apps (Jupyter, etc.)
- Job Composer with templates

**Source:** [OnDemand](https://docs.hpc.taltech.ee/ondemand.html) | [Visualization](https://docs.hpc.taltech.ee/visualization.html)

---

## 12. LUMI Access (Estonian users)

12-step process:
1. Login to <https://minu.etais.ee/> with MyAccessID
2. Choose affiliation `ttu.ee`
3. Identify with UniID
4. Agree and continue
5. Go to <https://mms.myaccessid.org/profile/>, fill fields, submit
6. Register SSH key in MyAccessID
7. Contact `hpcsupport@taltech.ee` (project leaders) or your project leader (team members)
8. Wait for HPC Centre response
9. Project appears in ETAIS account
10. Receive username from `info-noreply@csc.fi`
11. Connect: `ssh LUMIusername@lumi.csc.fi`
12. Read [LUMI docs](https://docs.lumi-supercomputer.eu/firststeps/getstarted/)

**Source:** [LUMI Quick Start](https://docs.hpc.taltech.ee/lumi/start.html)

---

## 13. Acknowledgement & Citation

When publishing results from TalTech HPC, include:
> "The simulations were carried out in the High Performance Computing Centre of TalTech."

Cite: Herrmann, Kaevand & Anton, "BASE: TalTech's HPC Infrastructure 2020–2024", TalTech Data Repository, Mar 2025. doi: [10.48726/zf23z-8ba50](https://data.taltech.ee/doi/10.48726/zf23z-8ba50)

**Source:** [Acknowledgement](https://docs.hpc.taltech.ee/acknowledgement.html)
