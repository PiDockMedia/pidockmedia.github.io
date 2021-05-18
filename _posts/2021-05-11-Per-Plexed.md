---
layout: post
title: Per-Plexed
---

## Synology Plex & Co

My latest Plex adventure. I have been down this road many times. I did a delivery system on a Mac Mini circa 2006 that eventually became Plex. A Linux desktop. A Linux home built NAS. On a drobo (What a fail). Plexguide on a cheap cloud instance. Linux again on an Intel NUC. I've learned a little ansible. I got docker down. I stuck everything behind Cloudflare. So. Many. Moving. Parts.

I originally names this entry "per-plexed" because I thought it sounded funny. But:

My orignal plan was to do it all with Native Apps but I have to be honest, as much as I HIGHLY appreciate the work the Synocommunity does, the permissions Nightmare that is inherent to the Synology, though something you can overcome, is just too complex for getting all the moving parts to work. I ended up having to set all permissions to 777 and even then, somehow, Sonarr did not have permissions to grab files downloaded with SABnzbd that had worked literally hours before with no changes, no restarts, so dicernable issues. It was maddening. So I am leaving the little bit below abut Plex native as I am keeing that for performance but I will have another doc for going back to docker for everything else.

I am loosely following this:

<https://keestalkstech.com/2019/11/docker-on-synology-from-git-to-running-container-the-easy-way/


- **Plex** - Native Install
	- Why Native? Proven hardware accelleration benefits.
	- Download the spk file from plex.tv (newer than Synology).
	- Download the plex.tv certificate as well
	- Open Package Center in DSM
	- Add the certificate.
		- Settings
			- Allow installation of packages published by "Synology Inc. and trusted publishers".
			- Certificate
				- Import
		- Click Manual Install and install the downloaded package.
		- Open DSM Control Panel and choose Shared Folder.
		- Select the Media Share and Edit.
		- On the Permissions tab give the plex user permissions.
			- Read Only is all that is NEEDED (common).
			- Read Write would allow you to delete from Plex if desired (uncommon).
		- Open Plex via the Package Center or directly (http://<nasname or ip>:32400/web).
		- Configure Plex (future link here for advanced config).
			- Add Movies library with Movies share.
			- Add TV library with TV.
