# Text to Talk: 3D Voice Avatar

A prototype that combines Google Cloud Text-to-Speech with a Three.js 3D avatar. The Express API can synthesize text into an MP3, and the browser code analyzes audio levels to move the avatar's lip meshes.

![3D avatar preview](./public/model-screen.png)

## What is included

- Express endpoint: `POST /speak` accepts JSON with a `text` value.
- Google Cloud Text-to-Speech generates an MP3 using the configured English (US) neutral voice.
- The generated audio is written to `public/audio/output.mp3`.
- Three.js loads `public/avatar_testglb.glb` and looks for meshes named `Upperlips` and `Lowerlips`.
- Lip movement is driven by average audio frequency levels. This is amplitude-based animation, not phoneme-accurate or AI lip-sync.
- The page provides Speak and Reload buttons. It currently has no text-entry form; send text to the API separately.

## Technology

- Node.js
- Express
- Vite
- Three.js
- Google Cloud Text-to-Speech

## Requirements

- Node.js 18 or newer (required by the Vite 5 development server).
- A Google Cloud project with the Text-to-Speech API enabled.
- A service account JSON key authorized to use that API.

## Run locally

1. Clone the repository and enter its directory:

   ```sh
   git clone https://github.com/sheikhsaad-net/text2talk.git
   cd text2talk
   ```

2. Install dependencies:

   ```sh
   npm install
   ```

3. Put your Google service-account JSON file at `auth/credentials.json` in the project root. The server sets `GOOGLE_APPLICATION_CREDENTIALS` to this path. Keep the key outside `public/`, never commit it, and add it to your local Git ignore rules before using a real key. Do not paste credentials into source files or publish them.

4. Start the development servers:

   ```sh
   npm run dev
   ```

   Vite serves the page at [http://localhost:5173](http://localhost:5173), and the Express API listens at [http://localhost:3000](http://localhost:3000).

## Generate speech

Send a request to the Express API, for example:

```sh
curl -X POST http://localhost:3000/speak ^
  -H "Content-Type: application/json" ^
  -d "{\"text\":\"Hello from the 3D avatar.\"}"
```

The endpoint returns an `audioUrl` for the generated file. The current page's Speak button plays the MP3 loaded when the page starts; reload the page after generating a new file before playing it.

## Current prototype limitations

- There is no text-entry UI or browser workflow that submits text to `/speak`; call the endpoint separately.
- Lip motion follows audio energy and does not match spoken phonemes.
- The source currently expects a root-level `auth/credentials.json`, while the old README described a credentials file under the public directory. Keep real credentials private and use the root-level path the server expects.
- The repository does not currently provide a production build or deployment configuration.

## License

This project is provided under the MIT License. See [LICENSE](./LICENSE).
