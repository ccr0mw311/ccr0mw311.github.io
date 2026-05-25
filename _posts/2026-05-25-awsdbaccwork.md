---
title: "AWS ASEAN Partner DB Acceleration Workshop – Phase 2"
date: 2026-05-25
categories: [AI, Database]
tags: [AI, Database]
---

AWS ASEAN Partner DB Acceleration Workshop – Phase 2

On May 19, 2026, I had the opportunity to visit the Amazon Web Services office and attend the ASEAN Partner DB Acceleration Workshop – Phase 2: Database Migration and Modernization Track.

“Post Pic”

The workshop covered several topics, including Migration & Modernization Overview, Generative AI-assisted Schema Conversion, AWS Transform for SQL Server, RDS Advisor Overview, and hands-on labs focused on database migration using AWS tools.

For this blog, I want to focus on the Migration & Modernization workshop, especially the Generative AI-assisted Schema Conversion activity.

Hands-on Workshop Experience

We used AWS Session Manager to securely access Windows and Linux instances without needing direct SSH or RDP access. During the workshop, we installed Kiro, an Agentic AI tool designed to help developers through spec-driven development and AI-assisted workflows.

“Post Pic Kiro”

Below are the installation commands used during the session:
curl -fsSL https://cli.kiro.dev/install | bash
export PATH="$HOME/.local/bin:$PATH"
source ~/.bashrc
kiro-cli login --use-device-flow

To test Kiro, I asked it to identify the AWS Region of the EC2 instance I was using. It successfully detected that the instance was running in us-east-1.

From there, we started working on schema conversion tasks. I selected an Oracle database as the source and used prompts to guide Kiro through the migration setup process.

Some of the prompts I used were:
Tell me about the RDS databases in this account.
I want to use the Oracle database as a source.
The connection details are in Secrets Manager, and I’ve declared the Oracle service ID.
Find the S3 buckets in this region with a bucket name starting with “dms-sc”.

Kiro responded with the required resources and prepared the environment for AWS DMS schema conversion tasks.

The workshop also provided a prompt that automated prerequisite steps for the Oracle source database, including installing Oracle query tools, saving SQL scripts locally, and executing them automatically.
Kiro then guided the process of connecting to Oracle, saving the provided SQL scripts into files, and executing the required commands. Once completed, my Oracle source database was ready for schema conversion.

For the target database, I selected Amazon Aurora PostgreSQL. Using Kiro, I continued setting up the migration project and configuring the target environment. I also saw the AWS DMS Instance Profile that Kiro automatically created during the setup.

With the help of Kiro, AWS Database Migration Service (DMS), and AWS Schema Conversion Tool (AWS SCT), the migration workflow became much easier to manage and automate.

Overall

One of the biggest takeaways from the workshop is how Agentic AI tools like Kiro can simplify complex database migration activities. Traditionally, migration and schema conversion tasks require multiple manual steps, deep database expertise, and careful configuration.

With Kiro, much of the process becomes guided and automated through natural language prompts. It can help identify AWS resources, prepare source databases, execute scripts, configure migration services, and assist with schema conversion workflows.

As organizations continue modernizing legacy databases into cloud-native platforms like Amazon Aurora PostgreSQL, AI-assisted tools can significantly reduce setup time, improve accuracy, and help teams accelerate migration projects more efficiently.

