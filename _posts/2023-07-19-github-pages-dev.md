---
title: "Github Pages Development Efficiency"
categories:
  - blog
tags:
  - markdown
  - static-sites
excerpt: "Setting up a local Jekyll Environment built in the same container environment used by GitHub on Kubernetes"
---

## An option to solve differences in dependencies between local & remote jekyll environments for github pages

Creating a local Rancher environment for GitHub Pages site development involves leveraging Rancher to manage containers, which, in turn, allows you to create and manage Kubernetes clusters for running your GitHub Pages development environment. The process unfolds as follows:

Begin by ensuring Docker is installed on your system and then proceed with Rancher's local installation, following the provided instructions.

Within Rancher, generate a new Kubernetes cluster if not already done, and connect to it. From there, it's time to initiate a Kubernetes deployment using a custom Docker image that incorporates the same Jekyll versions and dependencies utilized by GitHub Pages.

Before proceeding, don't forget to craft your bespoke Docker image, following these steps:

```bash
# Craft a Dockerfile in your GitHub Pages site directory
FROM jekyll/jekyll:latest

# Incorporate any additional dependencies or configurations, if needed

# Build the Docker image
docker build -t my-github-pages-image .
```

Note: You'll want to replace "my-github-pages-image" with a name more fitting for your custom Docker image.

Once the Docker image is ready, deploy your GitHub Pages site to the Kubernetes cluster through the following command:

```bash
kubectl create deployment my-github-pages-site --image=my-github-pages-image
```

This command lays the foundation for a deployment named "my-github-pages-site," operating with the Docker image previously created. As a result, the deployment effectively oversees a container running your GitHub Pages site.

To make the Jekyll development server within the container accessible, it must be exposed as a Kubernetes service, executed with the command:

```bash
kubectl expose deployment my-github-pages-site --type=NodePort --port=4000
```

The service is thus exposed as a NodePort service, utilizing port 4000.

With the deployment and service set up, you can now access your GitHub Pages site through the NodePort. To find the assigned port, utilize the following command:

```bash
kubectl get svc
```

Locate the service named "my-github-pages-site," and you'll observe a port entry (e.g., 4000:XXXXX/TCP). Simply access the site through your web browser by navigating to **http://localhost:XXXXX**, where XXXXX denotes the assigned port.

Now, take full advantage of your Rancher-managed Kubernetes cluster to work on your GitHub Pages site locally. Any modifications made will automatically manifest in the development server.

Remember to stay vigilant about updating your custom Docker image regularly. This ensures that you're always leveraging the most current versions of Jekyll and its dependencies for your GitHub Pages site.

## More from me

Check out my [LinkedIn profile][linkedin-profile] for more info about me and my career, as well as my [GitHub][github-profile], where you'll find the personal projects I've been working on.

If you have questions, feel free to drop me a message.

[linkedin-profile]: https://www.linkedin.com/in/robertbogan/
[github-profile]:   https://github.com/robert-bogan
