# FoxyDoxxing

First, a disclaimer:

This is __not__ security software.  I take security very seriously.  I am absolutely not perfect; none of us are; myself especially.  I've learned to value peer review very highly, and this has not been put through that yet.  I would not advocate using this on anything sensitive yet, and you should take certain cautions in deployment.  This is a swiss-army knife tool I built to get a certain job done.  It's working quite well for me, and I only want it to get better.

## Setup

1.	After cloning this repo, `cd /path/to/FoxyDoxxing` and pull down the necessary submodules with
	
	`git submodule update --init --recursive`

1.	Run `./setup.sh` or pre-configure the Annex with a .json config file (see **Configure** for more info) with `./setup.sh /path/to/config.json`.
1.	Follow the prompts.

## Configure

You may create a .json config file with any of the following directives to suit your needs.

#### Configuration Directives

###### Local Directives

*	**ssh_root (str)**
	The full path to your SSH config

*	**annex_dir (str)**
	The full path to your local submission folder (which should not exist beforehand!)

*	**uv_server_host (str)**
	The Annex server's hostname (it can be localhost, but if your instance faces the Internet and has an IP, you should put it here.)

*	**uv_uuid (str)**
	The shortcode for the server

*	**foxydoxxing_client (dict)**
	Contains the data needed to build your twitter and gmail authentication.  You will need to get an access token, client secret, and all that beforehand.  See the Google Developer API console and Twitter API console for more information.

## Messaging

The Annex will broadcast the status of all tasks to connected web Frontend clients via websocket.

#### Format

Messages from the annex channel will have the following format:

	{
		"_id" : "c895e95034a4a37eb73b3e691e176d0b",
		"status" : 302,
		"doc_id" : "b721079641a39621e08741c815467115",
		"task_path" : "Twitter.screenshot_tweet.screenshot_tweet",
		"task_type" : "UnveillanceTask"
	}

The annex channel will also send messages acknowledging the status of the connection.  Developers can do with that what they will.  The `_id` field is the task's ID in our database, the `doc_id` field represents the document in question (where available).

#### Status Codes

*	**201 (Created)** Task has been registered.
*	**302 (Found)** Task is valid, and can start.
*	**404 (Not Found)** Task is not valid; cannot start.
*	**200 (OK)** Task completed; finishing.
*	**412 (Precondition Failed)** Task failed; will finish in failed state.
*	**205 (Reset Content)** Task persists, and will run again after the designated period.
*	**410 (Gone)** Task deleted from task queue.
