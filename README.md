import subprocess
import argparse
import os

def run_command(command):
    print(f"\n> {command}")
    result = subprocess.run(command, shell=True, capture_output=True, text=True)
    print(result.stdout)
    if result.stderr:
        print("Error:", result.stderr)

def main():
    parser = argparse.ArgumentParser(description="Add, Commit, and Push with Git using Python.")
    parser.add_argument("-m", "--message", type=str, help="Commit message", default="Update")
    parser.add_argument("-f", "--force", action="store_true", help="Force execution without confirmation")

    args = parser.parse_args()

    # Step 1: Show git status
    print("git status:")
    run_command("git status")

    # Step 2: Show planned commands
    commands = [
        "git add .",
        f"git commit -m \"{args.message}\"",
        "git push"
    ]

    print("\nPlanned commands:")
    for cmd in commands:
        print(f"> {cmd}")

    # Step 3: Confirm
    if not args.force:
        confirm = input("\nExecute these commands? (y/n): ").lower()
        if confirm != "y":
            print("Cancelled.")
            return

    # Step 4: Execute commands
    for cmd in commands:
        run_command(cmd)

if __name__ == "__main__":
    main()
