# Apify actors: other networks and chaining, created 14:30

Sources: public store API and actor build input schemas, queried 2026-11-13; docs.apify.com pages on webhooks, metamorph, actor-to-actor integrations, run-sync API. Prices are pay-per-event (PPE) at the FREE plan tier unless noted. Monthly runs = public succeeded runs in the last 30 days.

## Live tests (2 of 3 used, operator identity only, github.com/andb)
- apivault_labs/github-profile-scraper with one profile URL: 5 s, 1 item, about 0.002 USD, no cookie. Field names: username, fullName, bio, company, location, website, email, twitterUsername, linkedin, socialAccounts, socialLinks, followers, following, publicRepos, avatarUrl, profileUrl, accountCreated, accountType, bestContact, plus lead-scoring fields (leadScore, leadTier, leadReasons, outreachIcebreaker, developerTier, activityStatus), enrichmentWarnings and success.
- apify/website-content-crawler on the same GitHub profile URL, maxCrawlPages 1, cheerio: 4 s, 1 item, about 0.002 USD. Field names: url, text, markdown, metadata, crawl, html, htmlUrl, screenshotUrl, debug.
- Total spend for the two runs is under 0.01 USD (the account's monthly usage moved from 0.0387 to 0.0407 USD across the second call and my checks). Account plan is FREE with 5 USD monthly credit.

## GitHub
- [apivault_labs/github-profile-scraper](https://apify.com/apivault_labs/github-profile-scraper): public GitHub profiles with website, social links, public email and lead scores; also repo-stargazer and org modes. 0.0019 USD per profile plus 0.00005 start. No login or cookie. 2,956 runs per month, 11.8k total, updated 2026-10-24. Inputs: profileUrls, repoUrls, orgUrls, extractRepos, includeSocialAccounts, includeOrganizations, emailFromCommits, verifyEmail and more, none required. Tested: yes.
- [saswave/github-profile-scraper](https://apify.com/saswave/github-profile-scraper): profiles from a list or from a repo's stargazers. 0.002 USD per result at FREE tier plus start fee. Has an optional cookies input (not needed for public profiles per its schema, unverified). 59 runs per month but 23k total, updated 2026-09-13. Inputs: peoples_links, repo_link, cookies. Tested: no.
- Not shortlisted: louisdeconinck/github-email-scraper (23k total runs, email focus).

## Personal websites
- [apify/website-content-crawler](https://apify.com/apify/website-content-crawler): crawls a URL and returns clean text and markdown; no login. Pay per platform usage (no per-event price listed; the test cost about 0.002 USD for one page). 3.68M runs per month. Inputs: startUrls, maxCrawlPages, maxCrawlDepth, crawlerType (cheerio is the cheap mode, playwright for JavaScript sites). Tested: yes. Best fit for a website link, followed by an LLM step to pull out a bio.
- [6sigmag/fast-website-content-crawler](https://apify.com/6sigmag/fast-website-content-crawler): faster variant, 0.003 USD per page plus 0.0015 start. 8,013 runs per month, last updated 2026-02-12. Tested: no. Costs more per page than the Apify one in our one-page case; no reason to prefer it.

## Instagram
- [apify/instagram-profile-scraper](https://apify.com/apify/instagram-profile-scraper): public profile data (bio, counts, external link) from usernames. 0.0026 USD per profile at FREE tier; the about-account add-on is for paying users only. No login. 11.1M runs per month. Inputs: usernames (required), includeAboutSection. Tested: no. Needs a username, so only usable when the attendee gives an Instagram link.
- [data-slayer/instagram-user-info-scraper-cookieless](https://apify.com/data-slayer/instagram-user-info-scraper-cookieless): cookieless profile lookup, failed lookups not charged. Start fee 0.0005 USD plus a per-profile fee (tiered, value not read). 65k runs per month. Inputs: usernames (required), mode, max_concurrency. Tested: no.

## X / Twitter
- [xquik/x-profile-scraper](https://apify.com/xquik/x-profile-scraper): profile by handle or URL. 0.00015 USD per profile row, the cheapest found. No login stated. 98k runs per month, updated 2026-11-13. Inputs: twitterHandles, startUrls, userIds, mode, maxItems, optional tweets and followers toggles. Tested: no.
- [apidojo/twitter-profile-scraper](https://apify.com/apidojo/twitter-profile-scraper): profile plus first 40 tweets. 0.016 USD per profile query at every tier, about 100 times the xquik price. 55k runs per month. Inputs: twitterHandles, startUrls, getAboutData, maxItems. Tested: no.

## Facebook
- [premiumscraper/facebook-pages-profile-scraper](https://apify.com/premiumscraper/facebook-pages-profile-scraper): public pages and, per its title, personal profiles by username or URL. 0.003 USD per profile at FREE tier plus 0.00005 start. No cookie input in the schema. 14k runs per month, updated 2026-07-27. Inputs: username, facebook_urls. Tested: no. Personal-profile coverage is doubtful: Facebook hides most personal profiles from logged-out visitors, so expect pages only.
- [cleansyntax/facebook-profile-posts-scraper](https://apify.com/cleansyntax/facebook-profile-posts-scraper): profile details or posts by URL or ID. 0.006 USD per result. 46k runs per month (424k total), updated 2026-04-20. Inputs: endpoint (required), urls_text, ids_text, max_posts. Tested: no.
- Facebook is the weakest network for us; treat it as optional.

## YouTube
- [streamers/youtube-channel-scraper](https://apify.com/streamers/youtube-channel-scraper): channel page and its videos. 0.0013 USD per video at FREE tier; set maxResults to 1 to keep a profile lookup near 0.001 to 0.003 USD. No login. 2.13M runs per month. Inputs: startUrls (required), maxResults, maxResultsShorts, maxResultStreams. Tested: no. Only worth running when the attendee gives a channel link.

## Medium and Substack
- Medium: no dedicated actor found; the store search for "medium" returned only unrelated media tools. Use website-content-crawler on the Medium profile URL.
- [automation-lab/substack-scraper](https://apify.com/automation-lab/substack-scraper): posts and newsletter data. 0.005 USD start plus per-post events. 1,848 runs per month, updated 2026-11-09. Tested: no.
- [digispruce/substack-scraper](https://apify.com/digispruce/substack-scraper): newsletter and posts. 2,874 runs per month, 102k total, updated 2026-08-22. Tested: no.
- For a bio, website-content-crawler on the Substack About page is cheaper and simpler than either.

## Meetup and Luma attendee pages
- Both categories scrape events, not attendee lists: [matyascimbulka/luma-event-scraper](https://apify.com/matyascimbulka/luma-event-scraper) (0.00007 USD per event, inputs slugs and cities required, 1.4k runs per month, last updated 2026-04-09) and [filip_cicvarek/meetup-scraper](https://apify.com/filip_cicvarek/meetup-scraper) (0.001 USD per event, search by keyword and city, 3.8k runs per month). Tested: no.
- Finding: these do not answer "who attends". One candidate claims group members ([haketa/meetup-scraper](https://apify.com/haketa/meetup-scraper), 459 runs total); member lists on Meetup need a login, so skip. Attendee enrichment from event pages is not available through the store.

## Cookie or login needs
- Needing a cookie or login (out for us): saswave GitHub only if cookies are supplied (optional), igolaizola/facebook-profile-scraper (private and public mode implies login), dtrungtin/facebook-profile-scraper (not checked, old).
- Claimed no login: every actor marked above as shortlisted. Only the two tested ones are verified.

## Chaining on Apify
- Calling from our own code with the API client: start each actor with run-sync-get-dataset-items (one HTTP call returns items). Limit is 300 seconds, then 408; both tested actors returned in 4 to 5 seconds. Free plan allows it. Cost is only the actors' own charges.
- Async variant: start a run, poll or wait, read the dataset. Same cost, no 300 s limit. Free plan allows it.
- Webhooks: fire a POST to a URL when a run succeeds or fails; the only action is an HTTP POST (docs state this limitation). Useful if our app exposes a callback endpoint, otherwise polling is simpler. No separate fee found.
- Actor-to-actor integrations: Apify-side chaining where a finished run triggers another actor or saved task, using integration-ready actors. Fixed wiring in the console, hard to branch per attendee. No separate fee found, you pay each actor.
- Metamorph: a running actor turns into another actor with a new input, inside one run. Needs us to write and deploy our own orchestrator actor; the docs describe it as stopping the container and starting the new one. Costs compute for the orchestrator on top.
- Saved tasks: stored actor input presets, called by id. Free. Good for fixing defaults (maxCrawlPages 1, cheerio) outside the code.
- Orchestrator actor: our own actor that calls others. Costs compute and build effort, and needs a deployment. Not on the plan for a hackathon.

## Recommendation for Circle Up
- Call actors from the app's own code. Per attendee, branch in code on which links were given: GitHub link to the GitHub scraper, website link to website-content-crawler, X handle to xquik, Instagram handle to apify/instagram-profile-scraper; no link means no call. Run the branches in parallel with run-sync-get-dataset-items and a 60 to 90 s timeout; each call took about 5 s in the tests.
- Reason: the chain depends on which fields the attendee filled in, which code expresses in a few lines and Apify-side integrations cannot branch on per item. Webhooks, metamorph and orchestrator actors add a deployed component and give no gain at this scale.
- Use saved tasks only to pin actor inputs. Expected cost per attendee with all four links is roughly 0.0065 USD (0.0019 + 0.002 + 0.00015 + 0.0026), so about 750 fully linked attendees on the 5 USD free credit.
- Skip Facebook, YouTube, Medium, Substack, Meetup and Luma for the demo unless a link is given; none gives data the other four don't.
