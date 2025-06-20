# Armbian Multi-Image Builder

This repository contains scripts and configurations to automate the building of multiple Armbian images, initially focusing on Rockchip RK3588 based boards.

## Overview

The main script, `multi_armbian_builder.sh`, orchestrates the build process, allowing the generation of images for different combinations of:

*   Boards
*   Releases
*   Desktop Environments (for Desktop images)
*   Server Images (without a graphical environment)

It utilizes the [official Armbian build framework](https://github.com/armbian/build).

## Key Features

*   **Automation:** Executes multiple builds sequentially.
*   **Flexible Configuration (in Script):** Allows defining Board, Release, and Desktop variations directly in the script variables.
*   **Build Framework Management:** Automatically clones the `armbian/build` repository if it's not present locally in the `build/` directory.
*   **Custom Configuration & Customization:** Automatically copies custom configuration files (`rockchip-rk3588.conf`), image customization scripts (`customize-image.sh`), and post-processing scripts (`compress-checksum.sh`) from this repository to the correct locations within the Armbian framework, overwriting defaults where applicable.
*   **Optimized Cleanup:** Intelligently performs cleanup (`CLEAN_LEVEL`) between builds to save time (cleans more when the board changes, less when only the release or desktop changes).
*   **Reporting:** Records the success or failure of each individual build and presents a summary at the end.
*   **Fault Tolerance:** Continues to the next build even if a specific one fails.
*   **Logging:** Displays messages with timestamps to track progress.

## Prerequisites

1.  **Operating System:** A Linux system compatible with the Armbian framework (Debian/Ubuntu are recommended).
2.  **Bash:** The script is written in Bash.
3.  **Git:** Required to clone this repository and the Armbian framework.
4.  **Armbian Build Dependencies:** The Armbian build framework itself has its dependencies (like `docker`, `sudo`, build tools, etc.). Consult the [official Armbian Build Framework documentation](https://docs.armbian.com/Developer-Guide_Build-Preparation/) for the complete list and system setup instructions. **It is crucial to prepare your environment according to the Armbian instructions before using this script.**

## Configuration

1.  **Clone this repository:**
    ```bash
    git clone <YOUR_REPOSITORY_URL>
    cd <CLONED_DIRECTORY_NAME>
    ```
2.  **Adjust Build Parameters (Optional):**
    *   Open the `multi_armbian_builder.sh` file in a text editor.
    *   Modify the `BOARDS`, `RELEASES`, `DESKTOPS` arrays and other parameter variables (like `BRANCH`, `ROOTFS_TYPE`, etc.) at the beginning of the script to define the images you want to build.
3.  **Adjust Board Configuration (Optional):**
    *   If necessary, edit the `rockchip-rk3588.conf` file to change specific kernel, U-Boot, or patch settings for the RK3588 family.
*   **Adjust Image Customization (Optional):**
    *   Edit the `customize-image.sh` script to add commands (like installing packages, copying files, modifying settings) that will run *inside* the image during the build process (within the chroot environment).

## Usage

1.  **Make the script executable:**
    ```bash
    chmod +x multi_armbian_builder.sh
    ```
2.  **Run the script:**
    ```bash
    ./multi_armbian_builder.sh
    ```
3.  **Run with Log to File (Recommended):**
    To save the detailed output to a log file, use the `tee` command:
    ```bash
    ./multi_armbian_builder.sh | tee build_log_$(date +%Y%m%d_%H%M%S).txt
    ```

The script will:
*   Check for/clone the `build/` directory.
*   Copy custom configuration (`rockchip-rk3588.conf`), image customization (`customize-image.sh`), and post-processing (`compress-checksum.sh`) scripts into the `build/` directory structure.
*   Present a menu to choose between building Desktop, Server, or both types of images.
*   Start the selected build loops.
*   Print logs and the final summary to the console (and log file, if using `tee`).

## Output

*   Successfully built images will be found inside the `build/output/images/` directory.
*   The build summary will be displayed at the end of the script execution.

## Next Steps / Potential Improvements

*   Implement command-line arguments to define build variations (boards, releases, etc.) without editing the script.
*   Add explicit checks for Armbian Build Framework dependencies.
