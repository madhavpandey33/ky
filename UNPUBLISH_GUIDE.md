# NPM Package Unpublish Guide

This guide provides workflows for unpublishing versions of `@mad_devx/ky` from npm.

## ⚠️ Important Warnings

- **Unpublishing is permanent and irreversible**
- **Other projects depending on the unpublished version will break**
- **You cannot republish the same version number again**
- **Only unpublish within 24 hours of publishing for best practices**
- **Consider deprecating instead of unpublishing for widely-used packages**

## Method 1: GitHub Actions Workflow (Recommended)

### Prerequisites
1. Ensure you have `NPM_TOKEN` secret configured in your GitHub repository
2. The token must have publish/unpublish permissions for `@mad_devx/ky`

### Steps
1. Go to your repository on GitHub
2. Navigate to **Actions** tab
3. Find **"Unpublish Package from npm"** workflow
4. Click **"Run workflow"**
5. Fill in the required inputs:
   - **Version**: The exact version to unpublish (e.g., `1.1.1`)
   - **Confirm**: Type `CONFIRM` (case-sensitive)
6. Click **"Run workflow"**

The workflow will:
- ✅ Validate your confirmation
- ✅ Check if the package version exists
- ✅ Show unpublish details
- ✅ Perform the unpublish operation
- ✅ Verify the unpublish was successful
- ✅ Provide a summary

## Method 2: Manual Command Line

### Prerequisites
1. Ensure you're logged into npm: `npm whoami`
2. If not logged in: `npm login`
3. Verify you have unpublish permissions for the package

### Commands

#### Unpublish a specific version:
```bash
npm unpublish @mad_devx/ky@1.1.1 --force
```

#### Unpublish the latest version:
```bash
# First, check what the latest version is
npm view @mad_devx/ky version

# Then unpublish it
npm unpublish @mad_devx/ky@<version> --force
```

#### Unpublish entire package (use with extreme caution):
```bash
npm unpublish @mad_devx/ky --force
```

### Verification
After unpublishing, verify the version is gone:
```bash
npm view @mad_devx/ky@<version>
# Should return an error if successfully unpublished
```

## Method 3: Alternative - Deprecate Instead

If you want to discourage usage without breaking existing installations:

```bash
npm deprecate @mad_devx/ky@1.1.1 "This version has been deprecated. Please upgrade to the latest version."
```

## Recovery Options

### If you need to "republish" (not possible with same version):
1. **Increment version number** in `package.json`
2. **Make necessary fixes**
3. **Publish new version**: `npm publish`

### If you unpublished by mistake:
1. **Contact npm support** immediately (within 24 hours)
2. **Explain the situation** - they may be able to help in some cases
3. **Be prepared to increment version** as recovery is not guaranteed

## Best Practices

### Before Unpublishing:
- [ ] Check download statistics: `npm view @mad_devx/ky`
- [ ] Search for dependents on GitHub/npm
- [ ] Consider deprecation instead of unpublishing
- [ ] Notify users if the package is widely used
- [ ] Have a migration plan ready

### After Unpublishing:
- [ ] Update documentation
- [ ] Notify users through appropriate channels
- [ ] Prepare a fixed version if needed
- [ ] Monitor for issues from dependent projects

## Troubleshooting

### Common Errors:

**"Cannot unpublish package"**
- Check if you have the right permissions
- Verify you're logged in with correct account
- Ensure the version exists

**"Package not found"**
- Double-check the package name and version
- Verify the version was actually published

**"Unpublish failed"**
- Try again after a few minutes
- Check npm status page for service issues
- Contact npm support if persistent

### Getting Help:
- npm support: https://www.npmjs.com/support
- GitHub Issues: Create an issue in this repository
- npm documentation: https://docs.npmjs.com/unpublishing-packages-from-the-registry

## Security Notes

- Keep your `NPM_TOKEN` secure and rotate regularly
- Use workflow_dispatch (manual trigger) for unpublish workflows
- Always require explicit confirmation for destructive operations
- Monitor your package for unauthorized changes

---

**Remember**: Unpublishing should be a last resort. Consider all alternatives before proceeding.
