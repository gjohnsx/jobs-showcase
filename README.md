# Jobs Showcase

This repo contains a set of jobs that can be run locally using the `@trigger.dev/cli` and `@trigger.dev/express` packages. The jobs included in this repo are:

- **[Cron scheduled basic](https://github.com/triggerdotdev/jobs-showcase/blob/main/src/cronScheduledBasic.ts)** - A scheduled Job which runs at 2:30pm every Monday.
- **[Scheduled interval basic](https://github.com/triggerdotdev/jobs-showcase/blob/main/src/scheduledIntervalBasic.ts)** - Runs every 60 seconds, starting 60 seconds after this Job is first indexed.
- **[Delay example joke](https://github.com/triggerdotdev/jobs-showcase/blob/main/src/delayExampleJoke.ts)** - Logs a message to the console, waits for 5 minutes, and then logs another message.
- **[GitHub: issue reminder](https://github.com/triggerdotdev/jobs-showcase/blob/main/src/gitHubIssueReminder.ts)** - Sends a Slack message to a channel if a GitHub issue is left open for 24 hours.
- **[GitHub: new issue opened](https://github.com/triggerdotdev/jobs-showcase/blob/main/src/gitHubNewIssueOpened.ts)** - Runs when a new issue is opened on a repo you have admin rights to, once created, it will add a 'Bug' label to the issue.
- **[OpenAI: generate image](https://github.com/triggerdotdev/jobs-showcase/blob/main/src/openAIGenerateImage.ts)** - Generate an image from a prompt, using OpenAI.
- **[OpenAI: tell me a joke](https://github.com/triggerdotdev/jobs-showcase/blob/main/src/openAITellMeAJoke.ts)** - Generate a joke from a prompt, using OpenAI.
- **[Resend: send basic email](https://github.com/triggerdotdev/jobs-showcase/blob/main/src/resendSendBasicEmail.ts)** - Sends a basic email using Resend.
- **[Slack: post message](https://github.com/triggerdotdev/jobs-showcase/blob/main/src/slackPostMessage.ts)** - Posts a message to a Slack channel.

## Setup

You will need to create a `.env` file. You can duplicate the `.env.example` file and set your local (Dev 'server') `TRIGGER_API_KEY` value.

### Running

Each file in `src` is either a Job or a separate set of jobs that can be run separately. For example, the `src/cronScheduledBasic.ts` file can be run with:

```sh
npm run cronscheduledbasic
```

This will open up a local server using `express` on port 8080. Then in a <u>separate terminal window</u> you can run the `@trigger.dev/cli dev` command:

```sh
npm run dev:trigger
```

### Contributing guide

You can add a new file to `src` with it's own `TriggerClient` and set of jobs (e.g. `src/events.ts`)

```ts
import { TriggerClient, eventTrigger } from "@trigger.dev/sdk";
import { createExpressServer } from "@trigger.dev/express";
import { z } from "zod";

export const client = new TriggerClient({ id: "job-catalog" });

client.defineJob({
  id: "example-job-1",
  name: "Example Job 1",
  version: "0.1.0",
  trigger: eventTrigger({
    name: "example.one",
  }),
  run: async (payload, io, ctx) => {},
});

createExpressServer(client);
```

Then add a new script in [`package.json`](./package.json):

```json
{
  "scripts": {
    "events": "nodemon --watch src/events.ts -r tsconfig-paths/register -r dotenv/config src/events.ts"
  }
}
```
