# Getting CleanFrame onto my TV

This repo is a copy of [nthnclrk/CleanFrame](https://github.com/nthnclrk/CleanFrame) (MIT), with its full upstream history so updates can be merged later:

```sh
git remote add upstream https://github.com/nthnclrk/CleanFrame.git
git fetch upstream && git merge upstream/main
```

CleanFrame is a **Tizen web app for Samsung TVs** and was built and tested on The Frame (LS03D, Tizen 9.0). Other Samsung models may work but aren't guaranteed. Android/Google TV, LG and Roku need a different app.

Installation has to run **on a computer on the same Wi-Fi/LAN as the TV**. It can't be done from a cloud session. The full walkthrough is in [docs/agent-tv-setup-guide.md](docs/agent-tv-setup-guide.md). This is the short version.

## Checklist

- [ ] **0. Preview it first** (optional). Install Node 22.13+ and Python 3.10+, then run:
      `npm ci && npm run dev:public` and open http://localhost:4795/home/
- [ ] **1. Record TV details.** Settings → Support → About This TV gives the model and software version. Network Status gives the TV's IP. Also note your computer's local IP.
- [ ] **2. Turn on Developer Mode.** Apps → App Settings, then type **1 2 3 4 5** on the remote's number pad (the 123 button). Turn Developer Mode **On**, set Host PC IP to **your computer's IP**, then restart the TV. If it later refuses connections, switch it off at the wall for 10 seconds.
- [ ] **3. Install Samsung's tools.** Install [Tizen Studio + TV Extension + Samsung Certificate Extension](https://developer.samsung.com/smarttv/develop/getting-started/setting-up-sdk/installing-tv-sdk.html), then connect:
      `<tizen-studio>/tools/sdb connect <TV_IP>:26101` and `sdb capability`
- [ ] **4. Create certificates.** Tizen Studio → Tools → Certificate Manager → + → Samsung → TV. Sign in with your own Samsung account. Create an author certificate and a distributor certificate that includes your TV's DUID. Back up the author certificate.
- [ ] **5. Configure signing.** Generate your own package/app ID with the Tizen project wizard. Copy `signing.example.json` to `.local/signing.json` and fill in `sdk_path`, `profile`, `package_id` and `app_id`. Put `{"host": "<TV_IP>"}` in `.local/device.json`. Both files are git-ignored.
- [ ] **6. Build, install, launch:**
      ```sh
      npm run package      # signs dist/CleanFrame.wgt with your profile
      npm run tv:install   # pushes and installs over SDB
      npm run tv:launch
      ```
- [ ] **7. Test with the remote.** Check the shortcuts, the app chooser and that you can get back to Samsung Home.

You can stop after step 7 and open CleanFrame from the Apps row. Making the remote's **Home button** open CleanFrame (guide stages 7–8) is an undocumented, riskier change with no guaranteed recovery. Skip it unless you've read those stages fully.

## Easiest route: let a local agent drive it

Run Claude Code (or the desktop app with Remote Control) **on your computer** in this folder. Then paste the "First prompt for your agent" from [docs/agent-tv-setup-guide.md](docs/agent-tv-setup-guide.md#first-prompt-for-your-agent) and work through the stages one at a time.
