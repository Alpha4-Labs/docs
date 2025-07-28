# Git Directory Setup Documentation

## Overview
This project uses **directory-specific git repositories** to manage different applications independently. Each directory has its own git repository pointing to different remote repositories.

## Directory Structure & Git Configuration

### 📁 `/partners` Directory
- **Repository**: `https://github.com/Alpha4-Labs/partners.git`
- **Branch**: `master`
- **Purpose**: Partner Dashboard Application
- **Status**: Independent React/TypeScript/Vite app with Dark-Light theme
- **Files**: 78 files (30,398 lines of code)

### 📁 `/frontend` Directory  
- **Repository**: `https://github.com/Alpha4-Labs/sui-mvp.git`
- **Branch**: `frontend-only`
- **Purpose**: Main Alpha Points Application
- **Status**: Main user-facing React application
- **Files**: 144 files (49,423 lines of code)

## How It Works

### 🔧 **Independent Git Repositories**
Each directory is initialized as its own git repository with:
- Separate git history
- Independent remote origins
- Individual branch tracking
- Isolated commit history

### 📤 **Push Behavior**
- **From `/partners`**: All pushes go to `partners.git`
- **From `/frontend`**: All pushes go to `sui-mvp.git` (frontend-only branch)

## Common Git Commands

### Working in `/partners` Directory
```bash
cd partners
git add .
git commit -m "Partner dashboard update"
git push  # → pushes to Alpha4-Labs/partners.git
```

### Working in `/frontend` Directory
```bash
cd frontend
git add .
git commit -m "Frontend application update"
git push  # → pushes to Alpha4-Labs/sui-mvp.git (frontend-only branch)
```

## Verification Commands

### Check Current Git Configuration
```bash
# In either directory
git remote -v                    # Shows remote repository
git branch                       # Shows current branch
git status                       # Shows working directory status
```

### Expected Outputs

**In `/partners`:**
```
origin  https://github.com/Alpha4-Labs/partners.git (fetch)
origin  https://github.com/Alpha4-Labs/partners.git (push)
```

**In `/frontend`:**
```
origin  https://github.com/Alpha4-Labs/sui-mvp.git (fetch)
origin  https://github.com/Alpha4-Labs/sui-mvp.git (push)
```

## Important Notes

### ⚠️ **Directory Context Matters**
- Always ensure you're in the correct directory before running git commands
- Each directory maintains its own git state independently
- No cross-contamination between repositories

### 🚀 **Deployment Configuration**
- **Partners**: Ready for deployment at `partners.alpha4.io`
- **Frontend**: Main application at `sui-mvp.alpha4.io`

### 🔄 **Migration History**
- Partners directory was extracted from frontend (78 files migrated)
- Frontend directory was cleaned of partner-specific code
- Both applications now operate independently

## Troubleshooting

### If Git Commands Don't Work As Expected:

1. **Check Current Directory**
   ```bash
   pwd  # Should show either .../partners or .../frontend
   ```

2. **Verify Remote Configuration**
   ```bash
   git remote -v
   ```

3. **Re-initialize If Needed** (⚠️ Use with caution)
   ```bash
   git init
   git remote add origin <correct-repo-url>
   ```

### Common Issues:
- **Wrong repository**: Check you're in the correct directory
- **Push rejected**: Ensure you're on the correct branch
- **Merge conflicts**: Pull latest changes before pushing

## Setup History

### Initial Migration (Completed)
- ✅ Partners directory extracted and configured
- ✅ Frontend directory cleaned and configured  
- ✅ Independent git repositories established
- ✅ Remote origins configured correctly
- ✅ Initial commits and pushes successful

### Branch Structure
- **Partners**: `master` branch (main development)
- **Frontend**: `frontend-only` branch (clean frontend code)

## Contact & Support
If you encounter issues with this setup, refer to this documentation or check the git status in each directory to understand the current state.

---
*Last Updated: $(date)*
*Configuration by: Alpha4 Development Team* 