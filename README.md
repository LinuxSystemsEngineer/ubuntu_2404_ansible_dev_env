# Ubuntu Linux Setup with Ansible

## Overview
This project automates the setup of a Ubuntu 24.04 LTS system using Ansible. It includes tasks such as installing apt packages, creating directories, configuring user environments, and more. The playbook, called the `main.yml` file, runs multiple tasks in the `./tasks` directory.

## Requirements
- git
- python3

## Prerequisites
This document assumes you already have SSH keys installed and configured for your localhost and remote client machines.  
If you need help installing or configuring SSH keys, please refer to [ssh_key_guide](https://github.com/LinuxSystemsEngineer/ssh_key_guide).  

You should also install and activate a segmented Python environment with the required pip packages from the `requirements.txt` file. **Additionally, this document assumes you have a user named `local_account_name` on your localhost machine. If your username differs, please adjust the files under the `./tasks` directory accordingly.**

## Requirements.txt file contents

This project's `requirements.txt` file includes the `ansible-dev-tools` package. This package bundles a suite of essential tools for Ansible development, streamlining the setup process and providing a comprehensive environment for working with Ansible. The package includes tools such as:

* **ansible-builder:** Automates creating execution environments for Ansible Collections.
* **ansible-core:** The core Ansible automation engine.
* **ansible-creator:** For quickly generating Ansible content (collections, roles, etc.).
* **ansible-dev-environment:** Simplifies the installation of Ansible Collections and their dependencies.
* **ansible-lint:** Checks Ansible playbooks for best practices.
* **ansible-navigator:** Provides a text-based user interface (TUI) for interacting with Ansible.
* **ansible-sign:** Enables signing and verification of Ansible project directories.
* **molecule:** A framework for developing and testing Ansible content.
* **pytest-ansible:** A pytest plugin for using Ansible within tests.
* **tox-ansible:** A tox plugin to create a matrix of Python and Ansible versions for testing.

Including `ansible-dev-tools` in `requirements.txt` ensures that all necessary tools for Ansible development are readily available.

## Installation

1. **Install the required apt packages:**

    ```bash
    sudo apt update && sudo apt install git python3 python3-dev python3-venv -y
    ```

2. **Clone the repository:**

    ```bash
    git clone https://github.com/LinuxSystemsEngineer/ubuntu_2404_ansible.git
    ```

3. **Change directories to the newly cloned repository:**

    ```bash
    cd ubuntu_2404_ansible
    ```

4. **Create a segmented Python virtual environment:**

    ```bash
    python3 -m venv .venv
    ```

5. **Activate the virtual environment:**

    ```bash
    source .venv/bin/activate
    ```

6. **Install required pip packages from the `requirements.txt` file:**

    ```bash
    pip3 install -r requirements.txt
    ```

## Configuring the ./hosts file

The `./hosts` file is crucial for Ansible to connect to your target servers. Before running the playbook, you *must* configure this file to reflect the IP address of your remote client machine.

1. **Locate the hosts file:** The `hosts` file location is in the main directory of your Ansible project.

2. **Edit the hosts file:** Open the `./hosts` file using your favorite text editor (e.g., emacs, gedit, nano, notepad++, pico, vim, vscode).

    ```bash
    vim ./hosts
    ```

3. **Add or modify the server entry:** For example, you might see a section like this:

    ```ini
    [staging_servers]
    ubuntu_test_server ansible_host=192.168.1.11
    ```

    Replace `192.168.1.11` with the actual IP address of your remote client machine.

4. **Save your changes.**

5. **Test the connection using Ansible's ping module:**

    ```bash
    ansible staging_servers -m ping
    ```

    A successful response will look similar to:

    ```ini
    ubuntu_test_server | SUCCESS => {
      "changed": false,
      "ping": "pong"
    }
    ```

**Example ./hosts file:**

If your remote client's IP address is `10.0.0.20`, your `./hosts` file should contain:

```ini
[staging_servers]
ubuntu_test_server ansible_host=10.0.0.20
