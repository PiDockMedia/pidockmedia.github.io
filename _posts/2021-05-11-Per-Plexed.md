---
layout: post
title: Per-Plexed
---

## Synology Plex & Co

My latest Plex adventure. I have been down this road many times. I did a delivery system on a Mac Mini circa 2006 that eventually became Plex. A Linux desktop. A Linux home built NAS. On a drobo (What a fail). Plexguide on a cheap cloud instance. Linux again on an Intel NUC. I've learned a little ansible. I got docker down. I stuck everything behind Cloudflare. So. Many. Moving. Parts.

Time to try and simplify things by putting it all on a Synology. My orignal plan was to do it all with Native Apps but so many things just work so well with Docker.

Folder Structure

1. There are LOTS of ways you can do this. This is my way.
2. I handle torrents off site (advanced config below).
2. Create a shared folder named Media
3. Open File Station
4. In the Media share create three folders "Downloads". "Movies", "TV".
5. Under Downloads create 3 folders "Download Station", "Torrents", "Usenet"
6. In Download Station create "Movies", "ToFetch", "TV"
7. In Torrents Create "In", "Out"
8. In both Torrents/In and Torrents/Out create "Movies" and "TV"
9. In Usenet create "InProgress", "Movies", "TV

On another list I iked that that they gave a visual guide:

	Media
	    /Downloads
	       /Download Station
	           /Movies
	           /ToFetch
	           /TV
	       /Torrents
	           /IN
	              /Movies
	              /TV
	           /OUT
	              /Movies
	              /TV
	        /Usenet
	           /InProgress
	           /Movies
	           /TV
	    /Movies
	    /TV

- Plex - Native Install
	- Why Native? Proven hardware accelleration benefits
	- Download the spk file from plex.tv (newer version than Synology delivers).
	- Download the plex.tv certificate as well
	- Open Package Center in DSM
	- Add the certificate.
		- Settings
			- Allow installation of packages published by "Synology Inc. and trusted publishers".
			- Certificate
				- Import
		- Click Manual Install and install the downloaded package
		- Open DSM Control Panel and choose Shared Folder
		- Select the Media Share and Edit
		- On the Permissions tab give the plex user permissions
			- Read Only is all that is NEEDED (common)
			- Read Write would allow you to delete from Plex if desired (uncommon)
		- Open Plex via the Package Center or directly (http://<nasname or ip>:32400/web)
		- Configure Plex (future link here for advanced config)
			- Add Movies library with Media/Movies
			- Add TV library with Media/TV
			