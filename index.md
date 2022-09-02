# AZ-104: Microsoft Azure Administrator Certification Exam Notes

## Exam Sections

- Manage Azure identities and governance (15–20%)
  - Configure resource locks.
  - Manage resource groups.
- Implement and manage storage (15–20%)

- Deploy and manage Azure compute resources (20–25%)
  - Configure VM
    - Move VMs from one resource group to another.
- Configure and manage virtual networking (20-25%)
- Monitor and maintain Azure resources (10–15%)

### Resources

- Microsoft Learn Pathway: <https://docs.microsoft.com/en-us/certifications/azure-administrator/>
- Exam prep videos:

  - [https://docs.microsoft.com/shows/exam-readiness-zone/preparing-for-az-204-develop-azure-compute-solutions-1-of-5](https://docs.microsoft.com/shows/exam-readiness-zone/preparing-for-az-204-develop-azure-compute-solutions-1-of-5)

  - [https://docs.microsoft.com/shows/exam-readiness-zone/preparing-for-az-204-develop-for-azure-storage-segment-2-of-5](https://docs.microsoft.com/shows/exam-readiness-zone/preparing-for-az-204-develop-for-azure-storage-segment-2-of-5)

  - [https://docs.microsoft.com/shows/exam-readiness-zone/preparing-for-az-204-implement-azure-security-segment-3-of-5](https://docs.microsoft.com/shows/exam-readiness-zone/preparing-for-az-204-implement-azure-security-segment-3-of-5)

  - [https://docs.microsoft.com/shows/exam-readiness-zone/preparing-for-az-204-monitor-troubleshoot-and-optimize-azure-solutions-segment-4-of-5](https://docs.microsoft.com/shows/exam-readiness-zone/preparing-for-az-204-monitor-troubleshoot-and-optimize-azure-solutions-segment-4-of-5)

  - [https://docs.microsoft.com/shows/exam-readiness-zone/preparing-for-az-204-connect-to-and-consume-azure-services-and-third-party-services-segment-5-of-5](https://docs.microsoft.com/shows/exam-readiness-zone/preparing-for-az-204-connect-to-and-consume-azure-services-and-third-party-services-segment-5-of-5)
- [Sample Questions](https://www.notion.so/-Questions-c3d027c819d64e1cb06d524da9fd555e)
- [More Sample Questions](https://docs.microsoft.com/certifications/resources/az-104-sample-questions)

### Deploy and manage Azure compute resources

#### Azure Resource Terminology

- **resource** - A manageable item that is available through Azure. Some common resources are a virtual machine, storage account, web app, database, and virtual network, but there are many more.
- **resource group** - A container that holds related resources for an Azure solution. The resource group can include all the resources for the solution, or only those resources that you want to manage as a group. You decide how you want to allocate resources to resource groups based on what makes the most sense for your organization.
- **resource provider** - A service that supplies the resources you can deploy and manage through Resource Manager. Each resource provider offers operations for working with the resources that are deployed. Some common resource providers are Microsoft.Compute, which supplies the virtual machine resource, Microsoft.Storage, which supplies the storage account resource, and Microsoft.Web, which supplies resources related to web apps.
- **template** - A JavaScript Object Notation (JSON) file that defines one or more resources to deploy to a resource group. It also defines the dependencies between the deployed resources. The template can be used to deploy the resources consistently and repeatedly.
- **declarative syntax** - Syntax that lets you state "Here is what I intend to create" without having to write the sequence of programming commands to create it. The Resource Manager template is an example of declarative syntax. In the file, you define the properties for the infrastructure to deploy to Azure.

#### Resource Providers

- Each resource provider offers a set of resources and operations for working with an Azure service. For example, if you want to store keys and secrets, you work with the Microsoft.KeyVault resource provider. This resource provider offers a resource type called vaults for creating the key vault.
- The name of a resource type is in the format: {resource-provider}/{resource-type}. For example, the key vault type is Microsoft.KeyVault/vaults.

#### Resource Groups

- Guidelines to follow creating a resource group:
  - All the resources in your group should share the same lifecycle. You deploy, update, and delete them together. If one resource, such as a database server, needs to exist on a different deployment cycle it should be in another resource group.
  - You can add or remove a resource to a resource group at any time.
  - You can move a resource from one resource group to another group.
  - A resource group can be used to scope access control for administrative actions.
  - A resource can interact with resources in other resource groups. This interaction is common when the two resources are related but don't share the same lifecycle (for example, web apps connecting to a database).
- Resource Locks:
  - Read-Only lock, which prevent any changes to the resource.
  - Delete locks, which prevent deletion.

#### Azure Portal

- The Azure portal is great for performing single tasks and for seeing a quick overview of the state of your resources.
- The portal is a Graphical User Interface (GUI) that makes it convenient to locate the resource you need and execute any required changes. It also guides you through complex administrative tasks by providing wizards and tooltips.
- The portal does not provide any way to automate repetitive tasks. For example, to set up 15 VMs, you would need to create them one by one, completing the wizard for each VM. This can be time consuming, and is error prone for complex tasks.

#### Azure CLI

- The Azure CLI is a command-line program to connect to Azure and execute administrative commands on Azure resources. It runs on Linux, macOS, and Windows, and allows administrators and developers to execute their commands through a terminal or command-line prompt (or script!) instead of a web browser.

#### Azure Powershell

- Azure PowerShell is a module you add to PowerShell to let you connect to your Azure subscription and manage resources. Azure PowerShell requires PowerShell to function. PowerShell provides services like the shell window, command parsing, and so on. The Azure Az PowerShell module adds Azure-specific commands.
- Azure PowerShell is also available two ways: inside a browser via the Azure Cloud Shell, or with a local install on Linux, Mac, or Windows.
  - In either way there are two modes to choose from. You can use it:
    - In interactive mode, in which you manually issue one command at a time.
    - In scripting mode, where you execute a script that consists of multiple commands.

#### How to choose an administrative tool?

- There is approximate parity between the portal, the Azure CLI, and Azure PowerShell with respect to the Azure objects they can administer and the configurations they can create. They are also all **cross-platform**. This means you will typically consider several other factors when making your choice:

  - Automation: Do you need to automate a set of complex or repetitive tasks? Azure PowerShell and the Azure CLI support his, while Azure portal does not.
  - Learning curve: Do you need to complete a task quickly without learning new commands or syntax? The Azure portal does not require you to learn syntax or memorize commands. In Azure PowerShell and teh Azure CLI, you must know the detailed syntax for each command you use.
  - Team skillset: Does your team have existing expertise? For example, your team may have used PowerShell to administer Windows. If so, they will quickly become comfortable using Azure PowerShell.

#### Using JSON ARM Templates to Deploy Azure Infrastructure as Code

- ARM templates are JavaScript Object Notation (JSON) files that define the infrastructure and configuration for your deployment.
- You can deploy an ARM template to Azure in one of the following ways:
  - Deploy a local template.
  - Deploy a linked template.
  - Deploy in a continuous deployment pipeline.

- Benefits:
  - ARM templates allow you to automate deployments and use the practice of infrastructure as code (IaC). The template code becomes part of your infrastructure and development projects. Just like application code, you can store the IaC files in a source repository and version it.
  - ARM templates are idempotent, which means you can deploy the same template many times and get the same resource types in the same state.
  - Resource Manager orchestrates the deployment of the resources so they're created in the correct order. When possible, resources will also be created in parallel, so ARM template deployments finish faster than scripted deployments.
  - Resource Manager also has built-in validation. It checks the template before starting the deployment to make sure the deployment will succeed.
  - If your deployments become more complex, you can break your ARM templates into smaller, reusable components. You can link these smaller templates together at deployment time. You can also nest templates inside other templates.
  - In the Azure portal, you can review your deployment history and get information about the state of the deployment. The portal displays values for all parameters and outputs.
  - You can also integrate your ARM templates into continuous integration and continuous deployment (CI/CD) tools like Azure Pipelines, which can automate your release pipelines for fast and reliable application and infrastructure updates. By using Azure DevOps and ARM template tasks, you can continuously build and deploy your projects.
- Template Structure:
  - **schema**: A required section that defines the location of the JSON schema file that describes the structure of JSON data. The version number you use depends on the scope of the deployment and your JSON editor.
  - **contentVersion**: A required section that defines the version of your template (such as 1.0.0.0). You can use this value to document significant changes in your template to ensure your're deploying the right template.
  - **apiProfile**: An optional section that defines the collection of API versions for resource types. You can use this value to avoid having to specify API versions for each resource in the template.
  - **parameters**: An optional section where you define values that are provided during deployment. These values can be provided by a parameter file, by command-line parameters, or in the Azure portal.
  - **variables**: An optional section where you define values that are used to simplify template language expressions.
  - **functions**: An option section where you can define user-defined functions that are available within the template. User-defined functions can simplify your template when complicated expressions are used repeatedly in your template.
  - **resources**: A required section that defines the actual items you want to deploy or update in a resource group or a subscription.
  - **output**: An optional section where you specify the values that will be returned at the end of the deployment.
