![demo](droidbot/resources/dummy_documents/DroidBot-GPT-demo.gif)

# DroidBot-GPT

## About

DroidBot-GPT is a GPT-powered GUI task automator for Android.
It can control a smartphone app automatically based on a natural language task description.
It is built upon [DroidBot](https://github.com/honeynet/droidbot) with [ChatGPT](https://chat.openai.com/)-style APIs.

This repo is a naive baseline for mobile task automation that does not require any customization for the apps. It simply send the task and state to GPT and ask for the next action to perform.
A more advanced version (with app-specific memory injection and other optimizations) can be found at [AutoDroid](https://github.com/MobileLLM/AutoDroid).

Technical report of this repo:

[Hao Wen, Hongming Wang, Jiaxuan Liu, Yuanchun Li. "DroidBot-GPT: GPT-powered UI Automation for Android"](https://arxiv.org/abs/2304.07061)


## How to install

Make sure you have:

1. `Python` (both 2 and 3 are supported)
2. `Java`
3. `Android SDK`
4. Added `platform_tools` directory in Android SDK to `PATH`
5. ChatGPT API or similar

Then clone this repo and install with `pip`:

```shell
git clone https://github.com/MobileLLM/AutoDroid-0shot.git
cd AutoDroid-0shot/
pip install -e .
```

If successfully installed, you should be able to execute `droidbot -h`.

## How to use

1. Prepare:

    + An app to use. Download the `.apk` file to your host machine.
    + A device or an emulator connected to your host machine via `adb`.
    + Modify how to query your LLM (with your own API or key) at [here](https://github.com/MobileLLM/DroidBot-GPT/blob/09c0d5d380c508f244321e236edb5697c59983e3/droidbot/input_policy.py#L740C6-L740C6)

2. Start AutoDroid-0shot:

    ```
    droidbot -a <path_to_apk> -o output_dir -task <your_task>
    ```
    That's it! The options are mostly the same as [DroidBot](https://github.com/honeynet/droidbot) except for the new `-task` option, where you can specify any task you want to complete. For example,

    - Create a contact named Alice with phone number 1234567.
    - Book a table for 4 people on Saterday.
    - Send a message to Sam to have a chat tonight.
    - ...

Note that this tool is currently for research purpose only. It may perform unintended actions on your device. Please use at your own risk.

Enjoy!

