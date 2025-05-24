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

**Question 5: All the resources that the cluster creates with a new workshop project must use workshop as the name for grading purposes. Each workshop must enforce the following maximum constraints:**
* **The project uses up to 2 CPUs.**
* **The project uses up to 1 Gi of RAM.**
* **The project requests up to 1.5 CPUs.**
* **The project requests up to 750 Mi of RAM.**

Now we are starting a new topic, so it’s a good moment to clear the chat and start a new one. For this exercise our environment has been provisioned with a *[quota.yaml](https://github.com/dialvare/ols-vs-ocp-cert/blob/main/resources/quota.yaml)* template to be completed by the user to match the requirements, so we will need to attach it to the query too.

![Question 5.1](https://github.com/dialvare/ols-vs-ocp-cert/raw/main/images/A1.5-1.png)

![Question 5.2](https://github.com/dialvare/ols-vs-ocp-cert/raw/main/images/A1.5-2.png)

Although the full YAML file isn’t visible in the screenshot, it has been correctly modified to meet the memory and CPU requirements. Additionally, Lightspeed provides a brief explanation for each of the completed fields, making it easier for the user to understand.
However, as we saw earlier, when applying the resource, it assumed a different file name (*workshop-resourcequota.yaml*) instead of the one actually provided in the attachment (*quota.yaml*). In this case, if we were to simply follow the suggested steps, we wouldn’t be able to create the resource.
That said, the steps themselves are technically correct, so the fair assessment here would be to mark it as **Partially Correct** 🟡.

**Question 6: Each workshop must enforce constraints to prevent an attendee's workload from consuming all the allocated resources for the workshop:**
* **A workload uses up to 750m CPUs.**
* **A workload uses up to 750 Mi.**

This question is similar to the previous one, but this time using Limit Ranges. Since we’re still working with project constraints, we’ll continue using the same chat tab. As we did before, we’re going to attach the [limitrange.yaml](https://github.com/dialvare/ols-vs-ocp-cert/blob/main/resources/limitrange.yaml) file to our question so that OLS can modify it for us.

![Question 6.1](https://github.com/dialvare/ols-vs-ocp-cert/raw/main/images/A1.6-1.png)

![Question 6.2](https://github.com/dialvare/ols-vs-ocp-cert/raw/main/images/A1.6-2.png)

We’re in the same situation as the previous question. The steps are correct. The file is modified to meet the requirements, but when it’s applied, the original name of the attached file is not used. Given all that, we’re going to give it another **Partially Correct** 🟡 mark.

**Question 7: Each workshop must have a resource specification for workloads:**
* **A default limit of 500m CPUs.**
* **A default limit of 500 Mi of RAM.**
* **A default request of 0.1 CPUs.**
* **A default request of 250 Mi of RAM.**

For this question, the requirements above need to be included in the limitrange.yaml file that we attached earlier. Let’s test if, keeping the same chat session, OLS is able to understand that these requirements need to be added to the file provided in a previous query. Also it will be interesting to see if the response will return only the limitations indicated above or if the model is able to concatenate those in the output file provided in the previous question.

![Question 7.1](https://github.com/dialvare/ols-vs-ocp-cert/raw/main/images/A1.7-1.png)

![Question 7.2](https://github.com/dialvare/ols-vs-ocp-cert/raw/main/images/A1.7-2.png)

The response we received is somewhat what we expected based on the results from the previous questions. Lightspeed understands the requirements well and modifies the file accordingly, but it does not retain the original file name. It’s surprising—in a negative sense—that it hasn’t managed to consolidate all the requirements from this and the previous question into a single file. However, the response is technically correct, although not enough to fully address this question, so we’ll assign it **Partially Correct** 🟡 again.

**Question 8: Each workshop project must have this additional default configuration:**
* **A local binding for the presenter user to the admin cluster role with the workshop name.**
* **The workshop=project_name label to help to identify the workshop workload.**
* **Each workshop must accept traffic only from within the same workshop by using the label workshop=project_name, or traffic coming from the ingress controller by using the label policy-group.network.openshift.io/ingress: "".**

New topic: Networking. Therefore, a new OpenShift LightSpeed chat will be started. This is probably the most complicated question we’ve asked so far in this blog. It covers several different topics and requires a complex configuration. To address it, a network policy is required. The environment includes a sample *[networkpolicy.yaml](https://github.com/dialvare/ols-vs-ocp-cert/blob/main/resources/networkpolicy.yaml)* template, which we will attach to the question.

![Question 8.1](https://github.com/dialvare/ols-vs-ocp-cert/raw/main/images/A1.8-1.png)

![Question 8.2](https://github.com/dialvare/ols-vs-ocp-cert/raw/main/images/A1.8-2.png)

