---

title: "Container Advantage and Disadvantages "
description: "The introduction to this course."

---

# Container Advantages 

Every day, developers find new ways to put containerization to work to solve their challenges. There is no shortage of ways to use containerization, and every application will likely produce unique benefits. Here are some of the most common reasons developers decide to containerize:

- Portability
   - No discussion of containerization is complete without at least one mention of the motto, “write once, run anywhere.” Since a container bundles all dependencies, you can take your application just about anywhere without rebuilding it to account for a new environment.

   Also, the abstraction provided by containerization ensures that your container works the same way regardless of where you deploy it. That means you can take your app to the cloud, run it on in a VM, or go directly to bare metal. As long as the host operating system supports your containerization tools, you are ready to deploy with minimal hassle.
- Efficiency
    - Containerization is one of the most efficient methods of virtualization available to developers. Containers improve efficiency in two ways: they use all available resources, and they minimize overhead.

    - When properly configured, containers allow a host to take advantage of virtually all available resources. Isolated containers can perform their operations without interfering with other containers, allowing a single host to perform many functions.

    - Containers also remove the need for virtualized operating systems, hypervisors, and other bottlenecks typically introduced by virtualization techniques. Unlike VMs, which rely on their virtual kernel, containers use the host operating system’s kernel. This drastically reduces overhead and minimizes resource use.
- Agility
    - Containerization is a crucial tool for streamlining DevOps workflows. You can create containers rapidly, deploy them to any environment, where they can be used to solve many diverse DevOps challenges.

     When a task presents itself, you can quickly develop a container to handle the job. If it is no longer needed, you can automatically shut it down until it is needed again. This is a technique known as orchestration. Technologies like Kubernetes automate the process of coordinating, managing, scaling, and removing containers.

     You can think of Kubernetes as the conductor of your container orchestra. With the help of Kubernetes-coordinated containers, developers can rapidly respond to problems and spin up novel solutions without worrying about lengthy and complicated deployments.


- Faster delivery
    - How long does it take upgrades to go from concept to implementation? Generally, the bigger an application, the longer it takes to get any improvements implemented. Containerization solves this issue by compartmentalizing your application. You can divide even the most enormous beast of an application into discrete parts using microservices.

    - Microservices take apart much larger applications by segmenting pieces into containers. This division makes it much easier for developers to implement changes and deploy new code. You can change isolated areas of the application without affecting the whole.
- Improved security
    - The isolation introduced by containerization also provides an additional layer of security. Because containers are isolated from one another, you can be confident that your applications are running in their own self-contained environment. That means that even if the security of one container is compromised, other containers on the same host remain secure.

    - In addition to being isolated from one another, containers are also isolated from the host operating system and can only minimally interact with computing resources. All of this equates to an inherently more secure way to deploy applications.
- Faster app startup
    - Compared to other methods of virtualization such as VMs, containers are extremely lightweight. One of the many benefits of being lightweight is rapid startup times. Because a container doesn’t rely on a hypervisor or virtualized operating system to access computing resources, startup times are virtually instantaneous.

    - The only limiting factor is the application itself. With no substantial overhead to wait for, the only startup delay is from your code. Rapid startup is a great reason for frequent updates and improvements.


- Easier management
   - Containerization allows developers the versatility to operate their code in either a virtualized or bare-metal environment. Whatever the demands of deployment, containerization can rise to meet them. Should there be a sudden need to retool your environment from metal to virtual or vice versa, your containerized applications are already prepared to make the switch.

   - Containerized apps using microservices become so flexible that you can host certain elements on bare metal and deploy others to virtual cloud environments.

   - Thinking with containers allows developers to reconceptualize their available resources. That might mean squeezing an extra drop of processing from a machine at maximum capacity. Or it could mean finding that what before seemed like a resource limitation was simply an opportunity to innovate.


- Flexibility
   - Kubernetes offers a variety of tools that simplify container management, like rollbacks and upgrades, as part of the platform. It also handles installation. There are self-healing features you can use to attempt to recover failed containers, terminate containers that fail health checks, and constantly monitor your containers’ health and status.

   - Kubernetes also automates resource management. You can allocate each container a set amount of CPU and RAM to handle its tasks. Ultimately, managing containers with the help of a tool such as Kubernetes is leaps and bounds easier than traditional application management methods.


# Container Disadvantages

-  Not right for all tasks
   - containers are ideally suited to microservice-type application development -- an approach that allows more complex applications to be configured from basic building blocks, where each building block is deployed in a container and the constituent containers are linked together to form the cohesive application. The application's functionality can then be scaled by deploying more containers of the appropriate building blocks rather than entire new iterations of the full application.
- Grappling with dependencies 
  - Common VMs are extremely self-contained and each VM includes a unique operating system (OS), drivers and application components. VMs can also be migrated to any other system as long as a suitable hypervisor is available. By comparison, containers run on top of a physical OS, sharing much of the underlying OS kernel along with many libraries and binaries. Bittman explained that placing dependencies on containers that can limit portability between servers. For example, Linux containers under Docker cannot run on current versions of Windows Server.
- Weaker isolation
   - Hypervisor-based VMs provide a high level of isolation from one another because the system's hardware resources are all virtualized and presented to the VMs through the hypervisor. This means a bug, virus or intrusion could compromise one VM, but not carry over to other VMs.

   - Containers are weaker because they share an OS kernel and components and already have a deep level of authorization (usually root access in Linux environments) in order to run in the first place. As a consequence, flaws and attacks have a much greater potential to carry down into an underlying OS and over into other containers -- potentially propagating malicious activity far beyond the original event.

   - While container platforms are evolving to segregate OS rights and limit vulnerable security postures, Bittman explains that administrators can boost security now by running containers in a VM. For example, it's possible to set up a Linux VM on Hyper-V and install Docker containers on the Linux VM. Even if containers within the VM are compromised, the vulnerability will not extend outside of the VM -- limiting the scope of potential damage.
- Potential for sprawl
   - Where VM lifecycle management is important for hypervisor-based environments, lifecycle management is absolutely critical for containers. Containers can be spun up and duplicated at an astonishing rate. This is an important benefit of containers, but it's also possible to consume a vast amount of computing resources without truly realizing it. That's not bad if the application's constituent containers are spun down or deleted when they're no longer needed. But the costs to scale up a containerized application, and then forgetting to scale it back later, can impose significant (and unnecessary) cloud computing costs for the enterprise. Bittman noted that cloud providers love it -- they make money renting computing power -- and the onus is on users to watch how containers are deployed.

- Limited tools
   - The kind of tools needed to monitor and manage containers are still lacking in the industry. This is not a new phenomenon. The early days of hypervisor-based virtualization were marked by a shortage of suitable tools. And just as capable VM monitoring and management tools are now readily available, new tools are starting to appear for container management. These include Google's open source Docker management tools Kubernetes, DockerUI to replace Linux command line functions with a web-based front end, Logspout to route container logs to a central location and so on.


