# Can AI Master OpenShift? LightSpeed Takes the Red Hat OCP Certification

Large Language Models (LLMs) have made remarkable strides in recent years, and their integration as AI assistants in technical environments is quickly becoming part of everyday workflows. These tools are now being used to handle a growing range of complex tasks - so it’s only natural to wonder how far they can really go. [Red Hat OpenShift Lightspeed](https://www.redhat.com/en/technologies/cloud-computing/openshift/lightspeed) is no exception. This AI assistant built into OpenShift, was meant to simplify tasks, accelerate workflows, and help users become more productive within OpenShift clusters. 

But what if we took it a step further? What if we treated it as a potential candidate for the [Red Hat Certified OpenShift Administrator](https://www.redhat.com/en/services/training/red-hat-certified-openshift-administrator-exam) exam? This certification validates the ability to:

* Troubleshoot OpenShift clusters and applications.
* Configure Authentication and Authorization within the cluster. 
* Ensure application security using Secrets and Security Context Constraints (SCCs).
* Expose applications and protect network traffic with Network Policies and TLS security.
* Control Pod scheduling and limiting resource usage with Resource Quotas.
* Manage OpenShift cluster updates, workloads and operators.

In this blog, we will explore whether OpenShift Lightspeed can handle real-world certification questions. We’ll challenge it with tasks and scenarios similar to those found in the Red Hat Certified OpenShift Administrator exam and evaluate its performance. Is Lightspeed truly Red Hat's OpenShift expert? Let’s find out.

## Exploring Lightspeed: What It Can and Can’t Do yet

The OpenShift Lightspeed Operator is currently in Tech Preview, meaning it’s still under active development and subject to rapid change. As with any fast-evolving technology, it’s important to understand what’s available today while keeping an eye on the roadmap and the features that may arrive in the near future.

As of today, OpenShift Lightspeed can be configured to work with external LLM providers such as WatsonX, OpenAI or Azure OpenAI, as well as models hosted on Red Hat OpenShift AI (RHOAI) and RHEL AI. These models can be queried directly through the interface embedded within OpenShift to get responses based on the OpenShift Documentation. Currently, LightSpeed is capable of generating context-aware responses in combination with links to relevant sections of the official documentation. Additionally, Lightspeed provides concrete CLI commands to help users take action directly. 

However, Lightspeed is not currently capable of executing actions directly on the cluster. For now, its role is centered around guidance, productivity enhancement, and task acceleration rather than active interrogation. That said, this may change in the future with the introduction of features like cluster interaction, which could enable Lightspeed to understand and act on the live state of the cluster and its resources.

## Defining our Benchmark

Before diving into our experiment, let’s establish a few criteria for the test. We’ll be working with the latest available version of **OpenShift LightSpeed (v0.3.4)**, configured to run with the *Azure OpenAI gpt-4* model.

For the evaluation, we’ll select a curated set of questions from each major topic area covered by the certification. We'll start by feeding LightSpeed the original questions exactly as they appear in the exam. If needed, we’ll adjust the phrasing slightly to provide additional context and help the model better understand it. When relevant, we’ll also include attachments to enrich the prompt.

Now, how will we measure success? Each response will be graded based on its relevance and technical accuracy. If LightSpeed provides a clear and actionable set of steps and commands to solve the task, it earns a **Correct** (100%) mark. If the response offers helpful guidance or general direction although not fully detailed or precise for the specific question, it’ll be marked as **Partially Correct** (50%). And if the answer is off-topic, vague, or doesn’t address the question, it’ll be considered **Incorrect** (0%).

The bar to pass is **70%** correct answers — the same as the certification exam. Now it’s LightSpeed’s moment to shine!

## Putting Lightspeed to the Test

In this section, we will compile all the questions that have been asked to Lightspeed. For each question, we will explain the decisions made when formulating it (such as adding attachments, maintaining or clearing the chat session, etc.). Then, a screenshot of the response provided by Lightspeed will be included. Please note that, in order to avoid making the blog too long, non-essential paragraphs such as validation steps or links to documentation have been omitted. Finally, each question will conclude with an analysis of the response.

**Question 1: Create the groups with the specified users in the following table:**

| **Group**           | **User**           |
|---------------------|--------------------|
| **platform**        | **do280-platform** |
| **presenters**      | **do280-presenter**|
| **workshop-support**| **do280-support**  |

We’ll begin the first exercise by opening a blank chat session in OpenShift LightSpeed, without any prior context. We’ll paste the question and the table directly into the message box, which means the table’s structure will be lost. Still, we want to see whether OLS is able to correctly interpret the rows and columns, and whether the response is accurate.

![Question 1](https://github.com/dialvare/ols-vs-ocp-cert/raw/main/images/A1.1.png)

The response is perfect! Lightspeed was able to fully understand the question and correctly interpret the table format. Once the commands were executed, the groups and users were created and assigned properly. We’re definitely going to give this first question a **Correct** 🟢 mark.

**Question 2: Ensure that only users from the following groups can create projects. Because this exercise requires steps that restart the Kubernetes API server, this configuration must persist across API server restarts.**

| **Group**           |
|---------------------|
| **platform**        |
| **presenters**      |
| **workshop-support**|

For this second question, since it's directly related to the first one, we’ll continue using the same chat session to provide more context and ensure a better response. This time, the question is not about performing a specific action—like creating users in the previous case—but rather about stating what we want to achieve, leaving it up to Lightspeed to infer the necessary steps to accomplish it.

![Question 2.1](https://github.com/dialvare/ols-vs-ocp-cert/raw/main/images/A1.2-1.png)

![Question 2.2](https://github.com/dialvare/ols-vs-ocp-cert/raw/main/images/A1.2-2.png)

Once again, a perfect response. The answer begins by outlining the steps to remove project-creation permissions from all users. Then, that role is granted only to the groups listed in the table. Finally, the ClusterRoleBinding is patched to make the changes permanent.
For all these reasons, we’re assigning this answer another **Correct** 🟢 mark.

**Question 3: The workshop-support group requires the following roles in the cluster:**
* **The admin role to administer projects.**
* **A custom role that is provided in the groups-role.yaml file. You must create this custom role to enable support members to create workshop groups and to add workshop attendees.**

As we’re still working with the topic of groups and roles, we’ll continue using the same chat session. In this case, the question format is different: it's presented as two separate bullet points. The second question requires us to attach the *[groups-role.yaml](https://github.com/dialvare/ols-vs-ocp-cert/blob/main/resources/groups-role.yaml)* file to the query so that OpenShift Lightspeed can read it and apply the necessary modifications.

![Question 3.1](https://github.com/dialvare/ols-vs-ocp-cert/raw/main/images/A1.3-1.png)

![Question 3.2](https://github.com/dialvare/ols-vs-ocp-cert/raw/main/images/A1.3-2.png)

OLS suggests creating a new file called manage-groups.yaml, even though we had already attached a file with the same content but a different name. While the procedure works, it could have simply suggested applying the attached file instead of creating a new one with identical content. This seems to be because OLS does not analyze the name of the file attached to the query—only its content. Even though additional steps are being added, the process is correct and successfully answers the question, so we can assign it a **Correct** 🟢 mark.

**Question 4: The platform group must be able to administer the cluster without restrictions.**

This will be the last question about users, groups, and roles, so we’ll continue using the same chat session to ensure OLS has the full context from the previous three questions.

![Question 4](https://github.com/dialvare/ols-vs-ocp-cert/raw/main/images/A1.4.png)

This was a fairly simple question, so OLS had no trouble providing a correct answer. With that single command, the necessary role to manage the cluster is assigned. That’s four **Correct** 🟢 answers in a row!






