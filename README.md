![demo](droidbot/resources/dummy_documents/DroidBot-GPT-demo.gif)

# DroidBot-GPT

## About

DroidBot-GPT is an easy-to-use UI agent framework for Android.
It can control a smartphone app automatically based on a natural language task description.
It is built upon [DroidBot](https://github.com/honeynet/droidbot).

Implementing a domain-specific mobile agent is very simple with DroidBot-GPT. You only need to modify two files: `droidbot/input_policy.py` which specifies how to call LLM based on the task and UI state and `droidbot/device_state.py` which specifies how to convert the UI state to something that LLM (or other multi-modal models) can understand.

The policy implemented in this repo is a naive baseline for mobile task automation.
It simply converts the view list to text, and sends the text to GPT and ask for the next action to perform.
A more advanced version (with app-specific memory injection and other optimizations) can be found at [AutoDroid](https://github.com/MobileLLM/AutoDroid).

Technical report of this repo:

[Hao Wen, Hongming Wang, Jiaxuan Liu, Yuanchun Li. "DroidBot-GPT: GPT-powered UI Automation for Android"](https://arxiv.org/abs/2304.07061)


## How to install

Make sure you have:

1. `Python` (both 2 and 3 are supported)
2. `Java`
3. `Android SDK`
4. Added `platform_tools` directory in Android SDK to `PATH`
5. Access to an LLM/LVM/LMM (e.g. ChatGPT API)

Then clone this repo and install with `pip`:

```shell
git clone https://github.com/MobileLLM/DroidBot-GPT.git
cd DroidBot-GPT/
pip install -e .
```

If successfully installed, you should be able to execute `python start.py -h`.


## How to use

1. Prepare:

    + An app to use. Download the `.apk` file to your host machine.
    + A device or an emulator connected to your host machine via `adb`.
    + Set the OpenAI URL and API KEY with environment variables `GPT_URL` and `GPT_KEY`.

2. Start DroidBot-GPT:

    ```
    python start.py -a <path_to_apk> -o output_dir -task <your_task>
    ```
    
    That's it! The options are mostly the same as [DroidBot](https://github.com/honeynet/droidbot) except for the `-task` option, where you can specify any task you want to complete. For example,

    - Create a contact named Alice with phone number 1234567.
    - Book a table for 4 people on Saterday.
    - Send a message to Sam to have a chat tonight.
    - ...

After execution, the action trace and screenshots can be found in `output_dir/index.html`.

## How to design your own agent

1. Decide which LLM (or other foundation models) to use. Modify how to query the model in the `_query_llm` function in `droidbot/input_policy.py`.

2. Decide how you want to represent the app UI in `droidbot/device_state.py`. Each `DeviceState` instance is a snapshot of current smartphone UI, which include the view hierarchy (the `views` field), the screenshot (the `screenshot_path` field), etc.

3. Design how to query the foundation model with the UI representation. Modify the code in the `_get_action_with_LLM` function in `droidbot/input_policy.py`. The function takes the current state (`current_state` parameter), the previous actions (`action_history` parameter) and the task description (`self.task` field) as input, and its job is to decide which action to take in the next step.

To further improve the agent performance, you may consider optimize how you invoke the LLM(s) and/or train/fine-tune the LLM(s).

Enjoy!

