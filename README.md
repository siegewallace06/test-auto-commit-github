# Auto-Increment Counter GitHub Action

This repository contains a GitHub Action workflow that automatically increments a counter in `content.txt` and commits the changes directly to the main branch. The workflow is manually triggered for controlled execution.

## 📁 Repository Structure

```
.
├── .github/
│   └── workflows/
│       └── increment-counter.yml    # GitHub Action workflow
├── content.txt                      # File containing the counter
└── README.md                       # This documentation
```

## 🚀 How It Works

The workflow performs the following steps:
1. **Reads** the current counter value from `content.txt`
2. **Increments** the counter by 1
3. **Updates** the file with the new counter value
4. **Commits** the changes with a descriptive message
5. **Pushes** directly to the main branch

## ⚙️ Setup Instructions

### Step 1: Create a Personal Access Token (PAT)

Since the workflow needs to push commits to the main branch, you need to create a Personal Access Token with appropriate permissions:

1. **Go to GitHub Settings**:
   - Click your profile picture → Settings
   - Or visit: https://github.com/settings/profile

2. **Navigate to Developer Settings**:
   - Scroll down to "Developer settings" in the left sidebar
   - Click "Personal access tokens" → "Tokens (classic)"

3. **Generate New Token**:
   - Click "Generate new token (classic)"
   - Give it a descriptive name: `Auto-Increment-Counter-Action`
   - Set expiration (recommended: 90 days for security)

4. **Select Required Scopes**:
   ```
   ✅ repo (Full control of private repositories)
     ✅ repo:status (Access commit status)
     ✅ repo_deployment (Access deployment status)
     ✅ public_repo (Access public repositories)
     ✅ repo:invite (Access repository invitations)
     ✅ security_events (Read and write security events)
   ```

5. **Generate and Copy**:
   - Click "Generate token"
   - **⚠️ IMPORTANT**: Copy the token immediately - you won't see it again!

### Step 2: Add Token as Repository Secret

1. **Go to Repository Settings**:
   - Navigate to your repository
   - Click "Settings" tab (requires admin access)

2. **Access Secrets**:
   - In left sidebar: "Secrets and variables" → "Actions"

3. **Add Repository Secret**:
   - Click "New repository secret"
   - **Name**: `PAT_TOKEN`
   - **Secret**: Paste your Personal Access Token
   - Click "Add secret"

### Step 3: Configure Repository Permissions (if needed)

If you encounter permission issues:

1. **Repository Settings** → **Actions** → **General**
2. **Workflow permissions**:
   - Select "Read and write permissions"
   - Check "Allow GitHub Actions to create and approve pull requests"
   - Click "Save"

## 🔧 Running the Workflow

### Manual Trigger

1. **Navigate to Actions**:
   - Go to your repository on GitHub
   - Click the "Actions" tab

2. **Find the Workflow**:
   - Look for "Increment Counter" in the workflow list
   - Click on it

3. **Run Workflow**:
   - Click "Run workflow" button (top right)
   - Ensure "Branch: main" is selected
   - Click the green "Run workflow" button

4. **Monitor Execution**:
   - Click on the running workflow to see live logs
   - Each step will show detailed output

## 🔒 Security Considerations

### Why Use PAT Instead of GITHUB_TOKEN?

- **GITHUB_TOKEN**: Limited permissions, cannot push to main branch in many configurations
- **Personal Access Token**: Full control, can bypass branch protection rules
- **Best Practice**: Use PAT with minimal required scopes

### Token Security Tips

1. **Regular Rotation**: Set expiration dates and rotate tokens regularly
2. **Minimal Scopes**: Only grant necessary permissions
3. **Secure Storage**: Never commit tokens to code, always use secrets
4. **Monitor Usage**: Review token usage in GitHub settings

## 📊 Expected Behavior

### Before Running:
```
Current Counter: 1
```

### After First Run:
```
Current Counter: 2
```

### Commit Messages:
```
Auto-increment counter to 2
Auto-increment counter to 3
Auto-increment counter to 4
```

## 🛠️ Troubleshooting

### Common Issues

1. **Permission Denied**:
   - Verify PAT_TOKEN secret exists
   - Check token permissions include `repo` scope
   - Ensure repository allows Actions to write

2. **Token Not Found**:
   - Confirm secret name is exactly `PAT_TOKEN`
   - Check token hasn't expired

3. **Workflow Not Appearing**:
   - Ensure `.github/workflows/` directory structure is correct
   - Verify YAML syntax is valid

4. **Push Fails**:
   - Check branch protection rules
   - Verify token has sufficient permissions

### Debug Steps

1. **Check Workflow Logs**:
   - Actions tab → Click failed run → Review step outputs

2. **Verify File Content**:
   - Ensure `content.txt` follows expected format
   - Counter line must be: `Current Counter: [number]`

3. **Test Token Permissions**:
   - Try creating a simple test commit manually
   - Verify the token works with git operations

## 📝 Customization

### Modify Counter Format

To change the counter format, update the `sed` command in the workflow:

```yaml
# Current format: "Current Counter: X"
sed -i "s/Current Counter: .*/Current Counter: $new_counter/" content.txt

# Example custom format: "Count = X"
sed -i "s/Count = .*/Count = $new_counter/" content.txt
```

### Change Increment Value

To increment by a different amount:

```yaml
# Current: +1
new_counter=$((current_counter + 1))

# Example: +5
new_counter=$((current_counter + 5))
```

## 📄 License

This project is open source and available under the [MIT License](LICENSE).

---

**⚡ Quick Start**: Create PAT → Add as `PAT_TOKEN` secret → Run workflow from Actions tab!
