# Identity Based access with HCP Waypoint

![HCP Waypoint](https://www.waypointproject.io/_next/image?url=https%3A%2F%2Fwww.datocms-assets.com%2F58478%2F1696443708-waypoint-io-illustration-3x.png&w=3840&q=75)

> [!NOTE]
> Before we start, please note Waypoint is an app in beta status. So things can be a bit flakey, but it's about the potential of it.

## What we're doing to do

- Introduction
- Concepts
- Create first template
- Create first application

> [!WARNING]
> Please always prefix your Waypoint application name with your project name. The applications are namespaced in HCP but not in Terraform Cloud and duplicate workspace names are not allowed.

## Introduction

HCP Waypoint is designed to empower platform teams to define golden patterns and workflows for developers to enable management of applications at scale.
