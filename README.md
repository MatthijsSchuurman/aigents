# Aigents
LLMs in a padded Docker cell

Small wrapper scripts + Docker images to run various “code agents” (opencode, codex, claude, gemini) inside a Docker container, locked to whatever directory you start them in.

They get:
- Your code
- Your terminal
- Your API keys

So yes, this is exactly the sort of setup that can:
- `rm -rf` the wrong thing at the speed of autocomplete
- Helpfully “refactor” your entire project into a puzzle
- Exfiltrate anything in the mounted directory to an API endpoint without asking

Treat this as a **toy** and **do not** point it at anything you care about. If you wouldn’t hand a random junior root access, don’t hand it to a language model either.

---

## How it works

At a high level:

- `opencode`, `codex`, `claude`, `gemini` are small Bash wrappers.
- They:
  - Build a base image (`Dockerfile.base`) and a tool-specific image (`Dockerfile.opencode`, `Dockerfile.codex`, etc.) when you ask them to.
  - Start a container with:
    - `.env` variables for API keys
    - The `home/` directory mounted at `/home/aigent`
    - Your current working directory mounted as `/home/aigent/workdir`

You then talk to the relevant AIgent inside the container, which talks to the LLM APIs, which in turn tell the container what to do to your code.

---

## Usage

Assuming you cloned this repo to `~/aigents`:

```bash
~/opencode build
```

Now from any project directory you want to sacrifice:

```bash
cd ~/my-awesome-code
~/aigents/opencode
```

This starts the opencode agent in Docker, confined to that directory (plus ~/aigents/home/ in the home directory)

If you want a shell inside the tool image instead:

```bash
~/aigents/opencode shell
```

---

# Safety notes
Anything in the working directory is fair game for:
- Reading
- Wrecking
- Deleting
- Uploading to random places the LLM desires

You have been warned. The LLM has not.
