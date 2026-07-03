# Package Release

```{warning}
This page is scheduled to be publicly available in June, 2025.
```

This chapter is intended for collaborators who are responsible for releasing and maintaining Molass packages. It provides concise instructions for managing the package release process.

## About PyPI

The Python Package Index (PyPI) is the official repository for Python packages. It allows developers to publish and distribute their Python libraries to a global audience. Users can install these packages using tools like `pip`. For Molass, PyPI serves as the platform for releasing and maintaining its packages, ensuring they are easily accessible to the community.

## PyPI Account

To publish packages to PyPI, you need a PyPI account. This account is required to authenticate yourself as a trusted maintainer and to manage the packages you release.

### How to Create a PyPI Account
1. Go to the [PyPI website](https://pypi.org/).
2. Click on the **"Register"** button in the top-right corner.
3. Fill in the required details, including:
   - **Username:** Choose a unique username.
   - **Email Address:** Provide a valid email address for verification and notifications.
   - **Password:** Create a strong password.
4. Complete the CAPTCHA and click **"Register"**.
5. Verify your email address by clicking the link sent to your inbox.

Once your account is created and verified, you can use it to upload and manage Molass packages on PyPI.

## PyPI API Token

A **PyPI API token** is a secure way to authenticate yourself when uploading packages to PyPI. Instead of using your username and password, which can be less secure, the API token allows you to perform package uploads while keeping your credentials safe.

### How to Create a PyPI API Token
1. Log in to your [PyPI account](https://pypi.org/).
2. Click on your username in the top-right corner and select **"Account settings"**.
3. Scroll down to the **"API tokens"** section and click **"Add API token"**.
4. Provide a name for the token (e.g., "Molass Package Upload").
5. Under **"Scope"**, select the appropriate option:
   - **Entire account:** Allows the token to manage all your packages.
   - **Specific project:** Restricts the token to a specific package (recommended for security).
6. Click **"Add token"**.
7. Copy the generated token and store it securely. You won’t be able to view it again.

### Important Notes
- Keep your API token private and do not share it. If it is compromised, revoke it immediately from your account settings.
- Use this token when uploading packages to PyPI, as described in the next section.

```{note}
For the default GitHub Actions procedure, only one PyPI API token is required. This token should be securely stored as a repository secret and will be used by the workflow to authenticate uploads. Collaborators do not need direct access to this token, as it is securely stored in the repository secrets.
```

## PyPI Upload

### Default Procedure: Using GitHub Actions [^1]

The default and recommended procedure for uploading Molass packages to PyPI is through a GitHub Actions workflow. This method ensures consistency, security, and ease of use for all collaborators.

#### Steps:
1. Go to the `"Actions"` tab in the [Molass Library repository](https://github.com/biosaxs-dev/molass-library).
2. Select the `"Manual Upload Python Package to PyPI"` workflow.
3. Click the `"Run workflow"` button.

```{note}
This workflow script is placed in .github/workflows/upload_to_pypi.yml.

It also includes a step to add a version tag to the repository to ensure the existance of corresponding tags.
```

#### Notes for Collaborators:
- **Permissions:**  
  Ensure you have **write** or **admin** access to the repository to trigger the workflow.
- **PyPI API Token:**  
  The workflow uses a PyPI API token stored securely in the repository secrets. Collaborators do not need direct access to the token.
- **Accountability:**  
  If multiple collaborators are uploading packages, it is recommended that each collaborator has their own PyPI account and API token. These tokens can be added as separate secrets in the repository for better security and traceability.

```{note}
If you are a collaborator (e.g., Person B) and encounter issues running the workflow, ensure that:
1. You have the necessary permissions in the repository.
2. The workflow is properly configured to allow manual triggering (`workflow_dispatch`).
```

If the GitHub Actions workflow cannot be used, you can manually upload the package using Twine, as described below.

### Alternative Procedure: Using Twine
To build required files, do as follows in the repository root folder.

```none
python -m build
```

This will generate the distribution files in the `dist` subfolder.

To upload them to PyPI, you will need to provide your API token[^2] to authenticate the upload when running the following `twine` command:

```none
twine upload dist/*
```

```{note}
When following this alternative procedure, each collaborator should generate their own API token under their individual PyPI accounts to ensure accountability and security.
```

By following the steps outlined in this chapter, collaborators can securely and efficiently manage the release of Molass packages to PyPI. The default GitHub Actions workflow ensures consistency and ease of use, while the Twine procedure provides a reliable fallback option when needed.

[^1]: This workflow is kept in `.github/workflows/upload_to_pypi.yml` in the repository.

[^2]: This API token should be kept safely somewhere.

See also:
* <a href="https://twine.readthedocs.io/en/stable/">Twine</a>