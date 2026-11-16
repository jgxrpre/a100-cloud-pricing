# A100 cloud pricing: what one card really costs per hour, when 80GB earns its premium, and why 8-GPU minimums break the comparison

Vultr lists the A100 PCIe at a starting rate of **$1.29 per GPU-hour**, and you can rent it one card at a time with no reservation and no sales call. That second part matters more than the first. On Google Cloud the smallest A100 instance is eight GPUs, so a one-hour experiment carries an entry ticket of about $26.27 whether the job can use eight cards or not.

What follows is the actual per-hour math: what Vultr charges across its GPU and standard plans, why the advertised rate and the configured rate differ, how the four A100 variants split, and the workloads where renting an A100 at all is the wrong call.

## What Vultr charges for A100, and what $1.29 does and doesn't cover

Vultr's A100 product page advertises "Starting at $1.29/per hour" for the A100 PCIe and sends you to the console to deploy. That number is the entry point, not a quote for the machine you'll end up running.

The Cloud GPU line on Vultr's price list itemizes a 1-GPU A100 80GB PCIe configuration at **$2.40/hr** with 12 vCPU, 120 GB RAM, 1.40 TB of storage and 10 TB of bandwidth attached. An 8-GPU PCIe A100 node on the same list totals $19.178/hr, which is the same $2.40 per GPU. The distance between $1.29 and $2.40 is the CPU, memory, storage and bandwidth bundled around the card, and the console is the only place that shows the configured figure for the bundle you pick.

Budget with both numbers in view. A GPU-bound job on a light bundle sits near the advertised floor; the standard 1-GPU configuration is $2.40/hr, or about $57.60 for a full day. At the $1.29 starting rate the same day costs $30.96.

👉 [Check Vultr's current A100 PCIe hourly rate and specs](https://www.vultr.com/products/cloud-gpu/nvidia-a100?ref=9738262-9J)

## Vultr's Cloud GPU lineup

| GPU | Per-GPU hourly | Node or bundle | Notes | Rate |
| --- | --- | --- | --- | --- |
| A100 80GB PCIe (1 GPU) | $2.40 configured, from $1.29 advertised | 12 vCPU, 120 GB RAM, 1.40 TB storage, 10 TB bandwidth | Single card, no reservation required | [ Check the A100 rate](https://www.vultr.com/products/cloud-gpu/nvidia-a100?ref=9738262-9J) |
| HGX A100 80GB SXM (8-GPU node) | from $2.80/GPU/hr | 8× SXM on NVLink, sold as a node | "Starting at" reflects pre-paid or preemptible terms; confirm the on-demand rate | [ See the SXM specifications](https://www.vultr.com/products/cloud-gpu/nvidia-a100?ref=9738262-9J) |
| HGX H100 80GB SXM (8-GPU node) | from $2.30/GPU/hr; $2.99/GPU on-demand | 8-GPU node, 216 vCPU, 1,914 GB RAM | The $2.30 figure matches the preemptible rate, not self-serve on-demand | [ Compare GPU pricing](https://www.vultr.com/pricing/?ref=9738262-9J) |
| HGX B200 192GB (8-GPU node) | from $3.50/GPU/hr; public API lists $8.50/GPU on-demand | 8-GPU node, 248 vCPU, 2,826 GB RAM | Page and API disagree; verify before quoting | [ Check current GPU rates](https://www.vultr.com/pricing/?ref=9738262-9J) |
| L40S 48GB (1 GPU) | $1.67/hr | 16 vCPU, 180 GB RAM, 1.20 TB storage, 10 TB bandwidth | Same $1.67 per GPU at 1, 2, 4 and 8 GPUs | [ See the L40S configuration](https://www.vultr.com/pricing/?ref=9738262-9J) |
| A40 48GB (1 GPU) | $1.71/hr | 24 vCPU, 120 GB RAM, 1.40 TB storage, 15 TB bandwidth | 4-GPU config totals $6.849/hr | [ View all Cloud GPU plans](https://www.vultr.com/pricing/?ref=9738262-9J) |

One oddity worth flagging: on Vultr's own list, HGX H100 SXM starts at $2.30 per GPU-hour against $2.80 for HGX A100 SXM. Read that as a pricing-model difference rather than a straight bargain. The $2.30 lines up with preemptible capacity; the self-serve on-demand rate for that node is $2.99 per GPU. A100 SXM at $2.80 is a "starting at" figure under the same caveat. Get both on-demand numbers before concluding the newer card is cheaper.

## The rest of Vultr's plans

GPUs rarely run alone. You'll want a cheap CPU instance for preprocessing, a database, or a scheduler, and those sit on separate product lines.

| Plan family | Entry configuration | From | Rate |
| --- | --- | --- | --- |
| Cloud Compute, Regular Performance | 1 vCPU / 0.5 GB / 10 GB | $2.50/mo ($3.50 with IPv4) | [ Check Cloud Compute pricing](https://www.vultr.com/products/cloud-compute/?ref=9738262-9J) |
| Cloud Compute, High Performance (AMD) | 1 vCPU / 1 GB / 25 GB NVMe | $6.00/mo | [ See the AMD high-performance plans](https://www.vultr.com/products/cloud-compute/?ref=9738262-9J) |
| Cloud Compute, High Performance (Intel) | 1 vCPU / 1 GB / 25 GB NVMe | $6.00/mo | [ See the Intel high-performance plans](https://www.vultr.com/products/cloud-compute/?ref=9738262-9J) |
| Cloud Compute, High Frequency | 1 vCPU / 1 GB / 32 GB NVMe | $6.00/mo | [ Check High Frequency pricing](https://www.vultr.com/products/high-frequency-compute/?ref=9738262-9J) |
| Optimized, General Purpose | 1 vCPU / 4 GB / 30 GB NVMe | $30.00/mo | [ View Optimized Cloud Compute](https://www.vultr.com/pricing/?ref=9738262-9J) |
| Optimized, CPU Optimized | 1 vCPU / 2 GB / 25 GB NVMe | $28.00/mo | [ View CPU Optimized plans](https://www.vultr.com/pricing/?ref=9738262-9J) |
| Optimized, Memory Optimized | 1 vCPU / 8 GB / 50 GB NVMe | $40.00/mo | [ View Memory Optimized plans](https://www.vultr.com/pricing/?ref=9738262-9J) |
| Optimized, Storage Optimized | 1 vCPU / 8 GB / 150 GB NVMe | $75.00/mo | [ View Storage Optimized plans](https://www.vultr.com/pricing/?ref=9738262-9J) |

Every line scales well past its entry configuration. The point of listing entry prices is to size the supporting cast: a $6/mo High Frequency instance handling tokenization and eval runs costs less than three hours of the A100 it feeds.

## One GPU or eight: the number that hits your card

This is where per-GPU-hour pricing misleads. Google Cloud's `a2-highgpu-8g` (8× A100 40GB) lists at roughly $26.27/hr. AWS `p4d.24xlarge`, also eight A100 40GB, lists at roughly $21.96/hr. Azure's ND A100 v4 series likewise begins at eight GPUs per virtual machine and doesn't publish a directly comparable per-GPU figure.

Divide either total by eight and you land near $2.75 to $3.28 per GPU-hour, which sounds competitive with Vultr's $2.40. You cannot buy one-eighth of the instance. The minimum bill for a sixty-minute job is $21.96, and that's before storage and egress.

Run the same hour at Vultr's configured $2.40 rate and $21.96 buys about nine GPU-hours on one card. At the $1.29 starting rate it buys roughly twenty. If your fine-tune, eval sweep or inference benchmark fits inside 80 GB, the hyperscaler comparison charges you for seven idle cards for the duration of the job.

An eight-GPU node only wins when the job needs eight cards talking to each other. For everything else, a single rentable card plus a resumable checkpoint is the cheaper shape.

👉 [See Vultr's full Cloud GPU and compute price list](https://www.vultr.com/pricing/?ref=9738262-9J)

## 80GB or 40GB, and which A100 you're actually renting

All four A100 variants carry identical compute figures: 9.7 TFLOPS FP64, 19.5 TFLOPS FP32, 156 TFLOPS TF32, 312 TFLOPS FP16/BF16 dense (624 with sparsity). What separates them is memory, bandwidth, power and interconnect.

| Variant | Memory | Bandwidth | TDP | MIG slices |
| --- | --- | --- | --- | --- |
| A100 80GB SXM | 80 GB HBM2e | 2,039 GB/s | 400W | 7 × 10 GB |
| A100 40GB SXM | 40 GB HBM2 | 1,555 GB/s | 400W | 7 × 5 GB |
| A100 80GB PCIe | 80 GB HBM2e | 1,935 GB/s | 300W | 7 × 10 GB |
| A100 40GB PCIe | 40 GB HBM2 | 1,555 GB/s | 250W | 7 × 5 GB |

Vultr sells the 80GB PCIe and the 80GB SXM. There's no 40GB option on its A100 page, so on Vultr the choice is form factor and node size, not memory capacity. If 40GB is genuinely enough for you, that's a reason to look at a different provider rather than a reason to overpay here.

Whether 80GB is enough depends on the job. The A100 remains a practical card for fine-tuning models in the 7B to 70B range, for inference where 80 GB of headroom covers the context you actually serve, and for FP64 work in traditional HPC. A 40GB card forces sharding or quantisation on the same models and gives up 380 to 484 GB/s of memory bandwidth. Since the FLOPs are identical across all four SKUs, bandwidth and capacity are the only things you're paying to upgrade.

## MIG: seven isolated instances on one card

Vultr supports Multi-Instance GPU partitioning, which slices a single A100 into as many as seven instances with dedicated memory and compute and no performance interference between tenants. On the 80GB card each slice gets about 10 GB; on the 40GB card, about 5 GB.

The economics are straightforward for the right workload. A model server handling several small classifiers, an embedding service, or a shared dev environment for a team can keep one card busy instead of paying for six underused ones. Power draw and instance count drop together.

The limit is capacity. A 7B model in FP16 needs roughly 14 GB just for weights, so a 10 GB slice won't hold it. MIG suits small models, batch inference and isolation between users, not serving a mid-size LLM.

## Billing granularity and egress

Billing increments decide whether short jobs are cheap or wasteful. Runpod bills per second with no egress fees. Google Cloud bills per second with a one-minute minimum and charges for egress. AWS bills per second with a 60-second minimum and charges for egress. Azure bills per second and charges for egress.

Vultr's A100 page doesn't state a billing increment, and it doesn't spell out an egress policy either. What the 1-GPU configuration does include is 10 TB of bandwidth, which is the allowance to measure your dataset transfers against. If you plan to run ten-minute jobs in a loop, confirm the increment in the console before that pattern becomes a habit. If your training data lives in object storage elsewhere, price the transfer out before you price the GPU.

## The $300 credit, and what the coupon page actually says

Vultr's coupons page currently lists four new-customer offers: **FLY300VULTR** for $300 in free credit, **250VULTRFLY** for $250, **FLYTWOHUNDRED** for $200, and **VULTRMATCH**, which matches a first deposit dollar for dollar up to $100. All four carry the same fine print: promotional credit applies to select products only, cannot be combined with any other offers, new customers only, restrictions and terms apply.

What the page does not say is whether GPU instances count as a covered product. Treat the credit as a discount on the surrounding infrastructure until support confirms otherwise. At $2.40/hr, $300 is 125 GPU-hours if the A100 qualifies; if it doesn't, the same credit still covers a lot of CPU instances, storage and bandwidth around your training runs.

👉 [Check the current Vultr promo codes and credit terms](https://www.vultr.com/coupons?ref=9738262-9J)

## When renting an A100 is the wrong move

The A100 shipped in 2020 and sits two architectural generations behind Blackwell. That is exactly why it's cheap, and for workloads that fit its shape the price per useful hour is hard to beat.

Three cases where it doesn't fit:

- **You need FP4 or FP8 throughput, or a single card with far more than 80 GB.** Vultr's own list already carries HGX B200 192GB nodes. If your model needs the memory or the lower-precision formats, an A100 rental is money spent on the wrong generation.
- **You need a fixed multi-year rate.** On-demand hourly listings aren't that product. Reserved commitments of one to three years can cut 50–60% off list, but the discount varies by provider and sometimes reserved pricing comes out higher than on-demand for the same card, so it has to be checked provider by provider.
- **You need exactly 40 GB at the lowest rate.** Vultr doesn't list a 40GB A100. Paying 80GB prices for a workload that fits in 40GB is a straight overspend.

## Before you deploy

Match the rental to the job:

- **One model, fits in 80 GB, job finishes in hours** → a single A100 80GB PCIe instance, stopped when the job ends.
- **Several small models or several users sharing one card** → MIG partitioning on the 80GB card, sized so no slice needs more than 10 GB.
- **Training that needs 4–16 GPUs on NVLink** → an HGX A100 SXM node, priced against the H100 node's on-demand rate rather than its "starting at" figure.
- **Anything that only needs a fraction of a card** → the L40S at $1.67/hr or the A40 at $1.71/hr will usually beat an A100 you can't fill.

Two things to confirm in the console before your first long run. First, the configured hourly rate for the exact vCPU, RAM and storage bundle you select, because the $1.29 headline and the $2.40 standard configuration are different numbers and only one of them will appear on your invoice. Second, whether the promotional credit reaches GPU products, since that changes the first month's effective cost by a wide margin.

👉 [Deploy a single A100 80GB PCIe instance on Vultr](https://www.vultr.com/products/cloud-gpu/nvidia-a100?ref=9738262-9J)
