# AWS EC2 Cloud/VR Gaming

#### A guide to setting up an Amazon AWS EC2 G instance for Cloud/VR Gaming

Using Spot Instances, this setup will cost you around **$0.4 USD per hour**. On-Demand instances cost double and are not recommended. There is also an instance available for approximately **$0.7 USD per hour** which I highly recommend. The reason is explained further down the page.

#### Stay up-to-date with the Spot pricing: [g4dn.xlarge](https://instances.vantage.sh/aws/ec2/g4dn.xlarge?region=us-east-1&os=mswin&cost_duration=hourly&reserved_term=Standard.noUpfront) | [g4dn.2xlarge](https://instances.vantage.sh/aws/ec2/g4dn.2xlarge?region=us-east-1&os=mswin&cost_duration=hourly&reserved_term=Standard.noUpfront)

> Estimates by Vantage; prices may differ slightly from AWS.
> Specifications are listed there or as following:
- Intel Xeon Platinum 8259CL @2.50GHz (4 cores or 8 cores)
- NVIDIA Tesla T4 with 12GB VRAM (equivalent to RTX 2060?)
- 16GB or 32GB of RAM
- 30GB of System storage (can be adjusted more)
- 100GB or 200GB of NVMe temporary storage (mountable via Disk Manager)

### Prerequisites / Requirements:
- An active AWS Account. ([Register here](https://aws.amazon.com/free/))
- Some money or AWS credits.
- Your favorite desktop streaming software: [Parsec](https://parsec.app), [NICE-DCV](https://docs.aws.amazon.com/dcv/latest/adminguide/what-is-dcv.html), or [Desktop Vision](https://desktop.vision).
- [Tailscale](https://tailscale.com) or [ZeroTier](https://www.zerotier.com) for network tunneling to reduce latency and improve responsiveness.
- [Virtual Desktop](https://vrdesktop.net) software for VR gaming.

> In rare cases, you might receive a **Beta invitation** ([see image](https://github.com/Froze202/AWSEC2-VR-CloudGaming/blob/main/images/beta-invitation.png?raw=true)) when signing up for the first time. This will grant you **$200 AWS Credits** at no cost. Activate a standard account if you receive this invitation.

### !! REGION MATTERS !!
#### Latency is crucial. Ensure you select the closest available region when logged into your AWS account.
- Check your latency using this [tool](https://www.cloudping.cloud/aws). Ignore CloudFront CDN.
- Change your region by clicking the current region at the top right ([see image](https://github.com/Froze202/AWSEC2-VR-CloudGaming/blob/main/images/region-list.png?raw=true)), next to your AWS account name.

### AWS Service Quotas Requirements
Requesting a Service Quota increase may take a few hours to 2-3 business days, or sometimes just a few minutes. If your request is declined, try again at a later day or request in another region that is closest to you.

- *All G and VT Spot Instance Requests of 4.
- **Running On-Demand G and VT instances of 4.

> *I recommend requesting 8 vCPU quotas instead of 4 for a `g4dn.2xlarge` instance, which has 8 cores and 32 GB of RAM, compared to the `g4dn.xlarge` instance with only 4 cores and 16 GB of RAM, to avoid CPU bottlenecks due to its slow Intel Xeon cores @2.50GHz.
>
> **Only request this quota if you want to launch or test an instance immediately.

### Next steps ...

Once you have everything set up, do the following:

1. Sign in to AWS Console.
2. *Head over to Spot Requests on the left side of the pane.

> *You might need to click on the hamburger icon if you don't see the left pane section.

3. Click on `Request Spot Instances` at the top right.
4. Launch Parameters: Manually configure launch parameters
5. AMI configuration:
   - Search for AMI > Change `My AMIs` to `Amazon AMIs` > Search for `DCV-Windows`
   - Sort by Name > Choose the one with `NVIDIA-gaming` keyword with latest drivers.
   - `DCV-Windows-XXXX.X.XXXXX-NVIDIA-gaming-555.99-XXXX-XX-XXXXX-XX-XX.XXXX` for example.
6. Key pairs configuration:
   - Create a new key pair (opens a new tab) > `Create key pair` (top right)
   - Name your key pair > Key pair type: `RSA` > Private key file format: `PEM`
   - *Click on `Create key pair` and you should get a key pair file downloaded.
   - Go back to `Create Spot Fleet Request` tab on your browser.

> *Please store the key pair somewhere safe and don't lose it. It is needed for the Windows login credentials.

7. Target capacity: Total target capacity = 1 instance
   - Checkbox `Maintain target capacity` > Set `Interruption behavior` from `Terminate` to `Stop`
   - Checkbox `Set maximum cost for Spot Instances`
   - Set your max cost (per hour) to the Spot pricing you see at the top of this page and add +0.1 to the total.
8. Instance type requirements: `Manually select instance types`
   - Checkbox all the instances and `Delete` them.
   - *Add instance types > Type in filter `g4dn.xlarge` or `g4dn.2xlarge` depending on your service quotas.
   - Checkbox the Instance type you wanted and click `Select`

> *Requesting any other than those two will require additional service quotas and thus, the request will not be fulfilled.

9. Allocation strategy: Capacity optimized
10. Your fleet request at a glance > Double check everything and `Launch` when ready.
    - Make sure `Estimated hourly price` is consistent with your max cost per hour.

This is unfinished. More content will be added later.

Last updated: 07/02/2024 (Tuesday, July 2, 2024)
