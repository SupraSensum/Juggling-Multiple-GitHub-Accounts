# Juggling-Multiple-GitHub-Accounts
A brief guide on how to handle using multiple GitHub accounts on the same macOS user account (e.g. for work and personal)

This is somewhat of a modification of [this lesson](https://www.theodinproject.com/lessons/foundations-setting-up-git).

To work with multiple GitHub accounts, you need to set up separate SSH keys for each account and configure your Git settings to use the appropriate SSH key for the relevant repository. Here's how to adjust your setup:

1. **Create SSH Keys**: For each GitHub account, you need a unique SSH key. If you've already created one for your primary account, create another for your secondary account using:

   ```bash
   ssh-keygen -t ed25519 -C "your_email@example.com"
   ```

   When prompted, save it under a different name from the default, like `~/.ssh/id_ed25519_second`.

2. **Attach SSH Keys to GitHub Accounts**: Log into each GitHub account and add the respective SSH keys to each account by navigating to Settings -> SSH and GPG keys -> New SSH Key. Use the `cat ~/.ssh/id_ed25519_second.pub` command to print the public key, which you then add to your GitHub account.

3. **Configure SSH to Use Different Keys**: Create or modify the `~/.ssh/config` file to manage multiple SSH keys. Here’s an example configuration:

   ```
   # Default GitHub
   Host github.com
     HostName github.com
     User git
     IdentityFile ~/.ssh/id_ed25519

   # Second GitHub Account
   Host github-second
     HostName github.com
     User git
     IdentityFile ~/.ssh/id_ed25519_second
   ```

   Replace `github-second` with a custom alias for your second GitHub account.

4. **Configure Git Repositories**: For each local repository, configure Git to use the correct SSH key by setting the repository's remote URL to use the alias defined in the SSH config. For the second account, you'd use:

   ```bash
   git remote set-url origin git@github-second:username/repo.git
   ```

5. **Set Local Git Identity (Optional)**: If needed, configure the local Git identity for each repository to use different user names or emails, useful if your GitHub accounts are set up with different details:

   ```bash
   git config user.name "Your Name"
   git config user.email "yourname@example.com"
   ```

   This needs to be done in each repository where you want a specific identity (omit `--global` to set it locally).

6. **Verify Configurations**: Ensure that the SSH keys are working and that Git is correctly using them. You can test SSH connection for each account by using:

   ```bash
   ssh -T git@github-second
   ```

   And check that the local repository configuration is correct:

   ```bash
   git config user.name
   git config user.email
   ```

By following these steps, you can manage multiple GitHub accounts on the same machine, each with its own authentication and configuration settings.

Now, you'll still want to refer back to the [OG lesson](https://www.theodinproject.com/lessons/foundations-setting-up-git) to ensure you've covered all your bases.