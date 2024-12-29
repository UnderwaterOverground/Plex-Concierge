# Plex Concierge - A Guide to Overseerr ⤫ Custom GPT

## Table of Contents
- [Overview](#overview)
- [Alternatives](#alternatives)
- [Functionality](#functionality)
- [Prerequisites](#prerequisites)
- [Guide](#guide)
- [Notes](#notes)
- [Explanation](#explanation)
- [Ideas](#ideas)
- [Screenshots](#screenshots)

---

## Overview
This is a guide for how to create a media discovery and management chat interface by connecting Overseerr to ChatGPT's Custom GPT tools. You can use this yourself, share it with friends, or connect it to a chat application!

![[demo-images/Demo-Mobile-Request.png]]

---

## Alternatives
If you're looking for other chat-to-Plex tools, they exist! This is a new approach to an already-solved problem, with the benefit of a nice conversational flow.

- **Requestrr** - A Discord chatbot [(GitHub)](https://github.com/darkalfx/requestrr/)
- **Doplarr** - A Discord request bot [(GitHub)](https://github.com/kiranshila/Doplarr)
- **Presumably more**

---

## Functionality
- **Adding movies and shows to Plex via natural language**
    - "Hey, can you add Big Buck Bunny?"
- **Display trailer links for movies/shows discussed**
- **Disambiguates content with similar names**
    - → "Did you mean Point Break 1991, or Point Break 2015?"
- **Understanding whether you're asking to add 4K or non-4K content**
    - "Can you add The Boys in 4k?"
- **Offering progress updates and ETAs on downloads/requests**
    - "How long until The Simpsons is ready?"
    - → "There are a few episodes still downloading, and they should all finish in about 2 hours and 25 minutes"
- **Providing direct links to watch content on Plex**
- **Support for bulk requests**
    - "Add the dark knight, the Simpsons, and the Simpsons movie"
- **Handling recommendations or general questions about content**
    - "What are Clint Eastwood's best movies?"
    - → *(Results)*
    - "Thanks, can you add the top 3?"

---

## Prerequisites
- **A ChatGPT Plus subscription** (In order to create Custom GPTs)
- **Overseerr** (Configured with Sonarr and Radarr)
    - Your Overseerr instance must be available from the internet and have some sort of static URL/address.
    - Supports additional Sonarr and Radarr 4K instances, but not required.

---

## Guide
1. Go to the **[My GPTs](https://chatgpt.com/gpts/mine)** section of the ChatGPT website and click "Create a GPT". *(GPT Plus Subscription required)*
2. Click on the 'Configure'. Provide a name and description. Add an avatar if desired.
3. For 'Instructions', use the provided [gpt-instructions.md](../../raw/refs/heads/main/src/gpt-instructions.md) file (use the raw version with markdown formatting).
4. *(Optional)* Add conversation starters, such as:
    - "Request a new show or movie"
    - "Find recommendations for popular movies and shows"
5. Configure 'Capabilities':
    - **Web Search**: On
    - **Canvas**: Off
    - **DALL•E Image Generation**: Off
    - **Code Interpreter & Data Analysis**: Off- 
6. By this point, it should look something like this:
	1. ![[demo-images/Tutorial-001.png]]
7. Create a New Action:
    - **Authentication**: Choose "API Key". Retrieve the API key from Overseer's *Settings → General* tab.
    - **Schema**: Use the provided [overseerr-actions-schema.yml](../../raw/refs/heads/main/src/overseerr-actions-schema.yml).
        - ❗️ *Replace the server URL with your Overseerr URL.*
    - **Privacy Policy**: Add any link (required if sharing the GPT).
    - By this point, it should look something like this: 
	    - ![[demo-images/Tutorial-002.png]]
1. Hit the **'Update'** button in the top right.
2. Adjust privacy settings as desired. Use a private link for sharing; avoid making this public.
3. Done! Test and explore its features.

---

## Notes
- **Parameter Workarounds**: Some unusual instructions were added in the schema, e.g., making 'seasons' required and defaulting it to 'all' for TV shows. Fixes issues with missing parameters.
- **Search Behavior**: Initial search queries may fail before retrying correctly due to URL encoding errors. This does not impact functionality.
- **Debugging**: Use the GPT editor's testing conversations to inspect request/response payloads sent to the Overseerr API.
- **Custom GPT Limitations**: GPTs can have up to 30 actions. Simplify schemas for complex tasks. This guide uses only 2 actions (*search* and *request*).

---

## Explanation

As a bit of background for the steps I took to do this, in case you were curious to try it out yourself from scratch for this or another use case:
1. I read about Custom GPTs using [(OpenAI's guide)](https://help.openai.com/en/articles/8554397-creating-a-gpt)
2. I looked at [Overseerr's API Documentation](https://api-docs.overseerr.dev/) and checked that it had the sort of functionality I wanted to use (in this case, I wanted to search for movie and tv content, and make requests to add it to Plex)
3. I read about [GPT Actions](https://platform.openai.com/docs/actions/introduction)
4. I read the GPT Actions [Getting Started Guide](https://platform.openai.com/docs/actions/getting-started) 
5. I chatted with OpenAI's [Actions GPT](https://chatgpt.com/g/g-TYEliDU6A-actionsgpt), and asked to read the [OpenAPI Schema](https://api-docs.overseerr.dev/overseerr-api.yml) for Overseerr, and generate a new spec for just the /search and /request endpoints
6. I created the new Custom GPT, and created a new Action. 
	1. I configured additional settings for API authorization
	2. I provided the schema I received from the Actions GPT chat
	3. I updated the schema to point to my Overseerr instance, since it defaulted to localhost
7. I tuned the operation of the Custom GPT through a mixture of prompted-edits and manual edits to it's instructions. 
	1. After a period of tuning, it was getting a bit confused with conflicting instructions and edge-cases. I used a separate ChatGPT chat to ask for it to consolidate the instructions and provide the result as raw markdown, which I copied back into the instructions section of the custom GPT
8. I used the debugging functionality available in the Custom GPT editor's chat window to test out queries and inspect request/response payloads when things went wrong

---

## Ideas
- **Connect to Discord or WhatsApp** for easier sharing and accessibility.
- **Extend Functionality** by adding new actions:
    - **Deleting Content**:
        - `DELETE /media/{mediaId}`
    - **Fetching Ratings Data**:
        - `GET /movie/{movieId}`
        - `GET /tv/{tvId}`
    - **Fetching Similar Content**:
        - `GET /movie/{movieId}/similar`
        - `GET /movie/{movieId}/recommendations`
        - `GET /tv/{tvId}/similar`
        - `GET /tv/{tvId}/recommendations`
- **Integrate Sonarr/Radarr APIs** for enhanced library management:
    - Force re-downloads
    - Specify preferred releases
- **Deep Links for iOS Apps**: Some bugs exist with deep links in ChatGPT. Reported issues pending resolution.

---

## Screenshots

#### Adding a show and seeing download progress
![[demo-images/Demo-Progress.png]]


#### Adding a show that already exists
![[demo-images/Demo-Already-Exists.png]]


#### Adding 4K Content
![[demo-images/Demo-4K-Content.png]]


#### Checking Availability on Plex
![[demo-images/Demo-Availability-Check.png]]


#### Querying Download Progress
![[demo-images/Demo-Estimated-Time.png]]

---
