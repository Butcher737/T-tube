LICENSEhttps://www.youtube.com/embed/VIDEO_ID?playlist=VIDEO_ID&loop=1./mach mochitest --headless devtools/client/<tool>./mach mochitest devtools/client/debugger./mach mochitest -f chrome --tag devtoolsMystorius:gha-enterprise-docs-openid-connect/workspaces/codespaces-blankPOST /{tenant}/oauth2/v2.0/token HTTP/1.1               // Line breaks for clarity

Host: login.microsoftonline.com:443

Content-Type: application/x-www-form-urlencoded

scope=https%3A%2F%2Fgraph.microsoft.com%2F.default

&client_id=97e0a5b7-d745-40b6-94fe-5f77d35c6e05

&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer

&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg

&grant_type=client_credentialscurl -X GET -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbG...." 'https://graph.microsoft.com/v1.0/users'GET /v1.0/users HTTP/1.1

Host: graph.microsoft.com:443

Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...AppCreationScripts/AppCreationScripts.mdimport { Octokit } from "octokit";

const octokit = new Octokit({ 

  auth: 'YOUR-TOKEN',

});import { Octokit } from "octokit";

const octokit = new Octokit({ 

  auth: 'YOUR-TOKEN',

});import { Octokit } from "octokit";

const octokit = new Octokit({ 

  auth: process.env.TOKEN,

});import { App } from "octokit";

const app = new App({

  appId: APP_ID,

  privateKey: PRIVATE_KEY,

});

const octokit = await app.getInstallationOctokit(INSTALLATION_ID);import { Octokit } from "octokit";

const octokit = new Octokit({ });await octokit.request("GET /repos/{owner}/{repo}/issues", {

  owner: "github",

  repo: "docs",

  per_page: 2

});import { Octokit } from "octokit";

const octokit = new Octokit({ });await octokit.request("GET /repos/{owner}/{repo}/issues", {

  owner: "github",

  repo: "docs",

  per_page: 2

});await octokit.rest.issues.listForRepo({

  owner: "github",

  repo: "docs",

  per_page: 2

});await octokit.request("POST /markdown/raw", {

  text: "Hello **world**",

  headers: {

    "content-type": "text/plain",

    "x-github-api-version": "2022-11-28",
import { Octokit } from "octokit";

const octokit = new Octokit({ 

  auth: 'YOUR-TOKEN',

});

async function getChangedFiles({owner, repo, pullNumber}) {

  let filesChanged = []

  try {

    const iterator = octokit.paginate.iterator("GET /repos/{owner}/{repo}/pulls/{pull_number}/files", {

      owner: owner,

      repo: repo,

      pull_number: pullNumber,

      per_page: 100,

      headers: {

        "x-github-api-version": "2022-11-28",

      },

    });

    for await (const {data} of iterator) {

      filesChanged = [...filesChanged, ...data.map(fileData => fileData.filename)];

    }

  } catch (error) {

    if (error.response) {

      console.error(`Error! Status: ${error.response.status}. Message: ${error.response.data.message}`)

    }

    console.error(error)

  }

  return filesChanged

}

async function commentIfDataFilesChanged({owner, repo, pullNumber}) {

  const changedFiles = await getChangedFiles({owner, repo, pullNumber});

  const filePathRegex = new RegExp(/\/data\//, "i");

  if (!changedFiles.some(fileName => filePathRegex.test(fileName))) {

    return;

  }

  try {

    const {data: comment} = await octokit.request("POST /repos/{owner}/{repo}/issues/{issue_number}/comments", {

      owner: owner,

      repo: repo,

      issue_number: pullNumber,

      body: `It looks like you changed a data file. These files are auto-generated. \n\nYou must revert any changes to data files before your pull request will be reviewed.`,

      headers: {

        "x-github-api-version": "2022-11-28",

      },

    });

    return comment.html_url;

  } catch (error) {

    if (error.response) {

      console.error(`Error! Status: ${error.response.status}. Message: ${error.response.data.message}`)

    }

    console.error(error)

  }

}

await commentIfDataFilesChanged({owner: "github", repo: "docs", pullNumber: 191});

import { Octokit } from "octokit";

const octokit = new Octokit({ 

  auth: 'YOUR-TOKEN',

});

async function getChangedFiles({owner, repo, pullNumber}) {

  let filesChanged = []

  try {

    const iterator = octokit.paginate.iterator("GET /repos/{owner}/{repo}/pulls/{pull_number}/files", {

      owner: owner,

      repo: repo,

      pull_number: pullNumber,

      per_page: 100,

      headers: {

        "x-github-api-version": "2022-11-28",

      },

    });

    for await (const {data} of iterator) {

      filesChanged = [...filesChanged, ...data.map(fileData => fileData.filename)];

    }

  } catch (error) {

    if (error.response) {

      console.error(`Error! Status: ${error.response.status}. Message: ${error.response.data.message}`)

    }

    console.error(error)

  }

  return filesChanged

}

async function commentIfDataFilesChanged({owner, repo, pullNumber}) {

  const changedFiles = await getChangedFiles({owner, repo, pullNumber});

  const filePathRegex = new RegExp(/\/data\//, "i");

  if (!changedFiles.some(fileName => filePathRegex.test(fileName))) {

    return;

  }

  try {

    const {data: comment} = await octokit.request("POST /repos/{owner}/{repo}/issues/{issue_number}/comments", {

      owner: owner,

      repo: repo,

      issue_number: pullNumber,

      body: `It looks like you changed a data file. These files are auto-generated. \n\nYou must revert any changes to data files before your pull request will be reviewed.`,

      headers: {

        "x-github-api-version": "2022-11-28",

      },

    });

    return comment.html_url;

  } catch (error) {

    if (error.response) {

      console.error(`Error! Status: ${error.response.status}. Message: ${error.response.data.message}`)

    }

    console.error(error)

  }

}

await commentIfDataFilesChanged({owner: "github", repo: "docs", pullNumber: 191});
  },
https://developer.android.com/studio?authuser=0https://github.com/firebase/codelab-friendlychat-android-sL https://firebase.tools | bashecho "# T-tube" >> README.md

git init

git add README.md

git commit -m "first commit"

git branch -M main

git remote add origin https://github.com/Butcher737/T-tube.git

git push -u origin main// When running in debug mode, connect to the Firebase Emulator Suite.

// "10.0.2.2" is a special IP address which allows the Android Emulator

// to connect to "localhost" on the host computer. The port values (9xxx)

// must match the values defined in the firebase.json file.

if (BuildConfig.DEBUG) {

    Firebase.database.useEmulator("10.0.2.2", 9000)

    Firebase.auth.useEmulator("10.0.2.2", 9099)

    Firebase.storage.useEmulator("10.0.2.2", 9199)

}

});- name: Cache

  uses: actions/cache@v1.2.1

  with:
- name: Setup Node.js environment

  uses: actions/setup-node@v3.7.0

  with:

    # Set always-auth in npmrc.

    always-auth: # optional, default is false

    # Version Spec of the version to use. Examples: 12.x, 10.15.1, >=10.15.0.

    node-version: # optional

    # File containing the version Spec of the version to use.  Examples: .nvmrc, .node-version, .tool-versions.

    node-version-file: # optional

    # Target architecture for Node to use. Examples: x86, x64. Will use system architecture by default.

    architecture: # optional

    # Set this option if you want the action to check for the latest available version that satisfies the version spec.

    check-latest: # optional

    # Optional registry to set up for auth. Will set the registry in a project level .npmrc and .yarnrc file, and set up auth to read in from env.NODE_AUTH_TOKEN.

    registry-url: # optional

    # Optional scope for authenticating against scoped registries. Will fall back to the repository owner when using the GitHub Packages registry (https://npm.pkg.github.com/).

    scope: # optional

    # Used to pull node distributions from node-versions. Since there's a default, this is typically not supplied by the user. When running this action on github.com, the default value is sufficient. When running on GHES, you can pass a personal access token for github.com if you are experiencing rate limiting.

    token: # optional, default is ${{ github.server_url == 'https://github.com' && github.token || '' }}

    # Used to specify a package manager for caching in the default directory. Supported values: npm, yarn, pnpm.

    cache: # optional

    # Used to specify the path to a dependency file: package-lock.json, yarn.lock, etc. Supports wildcards or a list of file names for caching multiple dependencies.

    cache-dependency-path: # optional
    # A directory to store and save the cache

    path: 

    # An explicit key for restoring and saving the cache

    key: 
- name: Setup .NET Core SDK

  uses: actions/setup-dotnet@v3.2.0

  with:

    # Optional SDK version(s) to use. If not provided, will install global.json version when available. Examples: 2.2.104, 3.1, 3.1.x, 3.x, 6.0.2xx

    dotnet-version: # optional

    # Optional quality of the build. The possible values are: daily, signed, validated, preview, ga.

    dotnet-quality: # optional

    # Optional global.json location, if your global.json isn't located in the root of the repo.

    global-json-file: # optional

    # Optional package source for which to set up authentication. Will consult any existing NuGet.config in the root of the repo and provide a temporary NuGet.config using the NUGET_AUTH_TOKEN environment variable as a ClearTextPassword

    source-url: # optional

    # Optional OWNER for using packages from GitHub Package Registry organizations/users other than the current repository's owner. Only used if a GPR URL is also provided in source-url

    owner: # optional

    # Optional NuGet.config location, if your NuGet.config isn't located in the root of the repo.

    config-file: # optional

    # Optional input to enable caching of the NuGet global-packages folder

    cache: # optional

    # Used to specify the path to a dependency file: packages.lock.json. Supports wildcards or a list of file names for caching multiple dependencies.

    cache-dependency-path: # optional
    
    # An ordered list of keys to use for restoring the cache if no cache hit occurred for key

    restore-keys: # optionalLICENSEhttps://www.youtube.com/embed/VIDEO_ID?playlist=VIDEO_ID&loop=1./mach mochitest --headless devtools/client/<tool>./mach mochitest devtools/client/debugger./mach mochitest -f chrome --tag devtoolsMystorius:gha-enterprise-docs-openid-connect/workspaces/codespaces-blankPOST /{tenant}/oauth2/v2.0/token HTTP/1.1               // Line breaks for clarity

Host: login.microsoftonline.com:443

Content-Type: application/x-www-form-urlencoded

scope=https%3A%2F%2Fgraph.microsoft.com%2F.default

&client_id=97e0a5b7-d745-40b6-94fe-5f77d35c6e05

&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer

&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg

&grant_type=client_credentialscurl -X GET -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbG...." 'https://graph.microsoft.com/v1.0/users'GET /v1.0/users HTTP/1.1

Host: graph.microsoft.com:443

Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...AppCreationScripts/AppCreationScripts.mdimport { Octokit } from "octokit";

const octokit = new Octokit({ 

  auth: 'YOUR-TOKEN',

});import { Octokit } from "octokit";

const octokit = new Octokit({ 

  auth: 'YOUR-TOKEN',

});import { Octokit } from "octokit";

const octokit = new Octokit({ 

  auth: process.env.TOKEN,

});import { App } from "octokit";

const app = new App({

  appId: APP_ID,

  privateKey: PRIVATE_KEY,

});

const octokit = await app.getInstallationOctokit(INSTALLATION_ID);import { Octokit } from "octokit";

const octokit = new Octokit({ });await octokit.request("GET /repos/{owner}/{repo}/issues", {

  owner: "github",

  repo: "docs",

  per_page: 2

});import { Octokit } from "octokit";

const octokit = new Octokit({ });await octokit.request("GET /repos/{owner}/{repo}/issues", {

  owner: "github",

  repo: "docs",

  per_page: 2

});await octokit.rest.issues.listForRepo({

  owner: "github",

  repo: "docs",

  per_page: 2

});await octokit.request("POST /markdown/raw", {

  text: "Hello **world**",

  headers: {

    "content-type": "text/plain",

    "x-github-api-version": "2022-11-28",
import { Octokit } from "octokit";

const octokit = new Octokit({ 

  auth: 'YOUR-TOKEN',

});

async function getChangedFiles({owner, repo, pullNumber}) {

  let filesChanged = []

  try {

    const iterator = octokit.paginate.iterator("GET /repos/{owner}/{repo}/pulls/{pull_number}/files", {

      owner: owner,

      repo: repo,

      pull_number: pullNumber,

      per_page: 100,

      headers: {

        "x-github-api-version": "2022-11-28",

      },

    });

    for await (const {data} of iterator) {

      filesChanged = [...filesChanged, ...data.map(fileData => fileData.filename)];

    }

  } catch (error) {

    if (error.response) {

      console.error(`Error! Status: ${error.response.status}. Message: ${error.response.data.message}`)

    }

    console.error(error)

  }

  return filesChanged

}

async function commentIfDataFilesChanged({owner, repo, pullNumber}) {

  const changedFiles = await getChangedFiles({owner, repo, pullNumber});

  const filePathRegex = new RegExp(/\/data\//, "i");

  if (!changedFiles.some(fileName => filePathRegex.test(fileName))) {

    return;

  }

  try {

    const {data: comment} = await octokit.request("POST /repos/{owner}/{repo}/issues/{issue_number}/comments", {

      owner: owner,

      repo: repo,

      issue_number: pullNumber,

      body: `It looks like you changed a data file. These files are auto-generated. \n\nYou must revert any changes to data files before your pull request will be reviewed.`,

      headers: {

        "x-github-api-version": "2022-11-28",

      },

    });

    return comment.html_url;

  } catch (error) {

    if (error.response) {

      console.error(`Error! Status: ${error.response.status}. Message: ${error.response.data.message}`)

    }

    console.error(error)

  }

}

await commentIfDataFilesChanged({owner: "github", repo: "docs", pullNumber: 191});

import { Octokit } from "octokit";

const octokit = new Octokit({ 

  auth: 'YOUR-TOKEN',

});

async function getChangedFiles({owner, repo, pullNumber}) {

  let filesChanged = []

  try {

    const iterator = octokit.paginate.iterator("GET /repos/{owner}/{repo}/pulls/{pull_number}/files", {

      owner: owner,

      repo: repo,

      pull_number: pullNumber,

      per_page: 100,

      headers: {

        "x-github-api-version": "2022-11-28",

      },

    });

    for await (const {data} of iterator) {

      filesChanged = [...filesChanged, ...data.map(fileData => fileData.filename)];

    }

  } catch (error) {

    if (error.response) {

      console.error(`Error! Status: ${error.response.status}. Message: ${error.response.data.message}`)

    }

    console.error(error)

  }

  return filesChanged

}

async function commentIfDataFilesChanged({owner, repo, pullNumber}) {

  const changedFiles = await getChangedFiles({owner, repo, pullNumber});

  const filePathRegex = new RegExp(/\/data\//, "i");

  if (!changedFiles.some(fileName => filePathRegex.test(fileName))) {

    return;

  }

  try {

    const {data: comment} = await octokit.request("POST /repos/{owner}/{repo}/issues/{issue_number}/comments", {

      owner: owner,

      repo: repo,

      issue_number: pullNumber,

      body: `It looks like you changed a data file. These files are auto-generated. \n\nYou must revert any changes to data files before your pull request will be reviewed.`,

      headers: {

        "x-github-api-version": "2022-11-28",

      },

    });

    return comment.html_url;

  } catch (error) {

    if (error.response) {

      console.error(`Error! Status: ${error.response.status}. Message: ${error.response.data.message}`)

    }

    console.error(error)

  }

}

await commentIfDataFilesChanged({owner: "github", repo: "docs", pullNumber: 191});
  },
https://developer.android.com/studio?authuser=0https://github.com/firebase/codelab-friendlychat-android-sL https://firebase.tools | bashecho "# T-tube" >> README.md

git init

git add README.md

git commit -m "first commit"

git branch -M main

git remote add origin https://github.com/Butcher737/T-tube.git

git push -u origin main// When running in debug mode, connect to the Firebase Emulator Suite.

// "10.0.2.2" is a special IP address which allows the Android Emulator

// to connect to "localhost" on the host computer. The port values (9xxx)

// must match the values defined in the firebase.json file.

if (BuildConfig.DEBUG) {

    Firebase.database.useEmulator("10.0.2.2", 9000)

    Firebase.auth.useEmulator("10.0.2.2", 9099)

    Firebase.storage.useEmulator("10.0.2.2", 9199)

}

});- name: Cache

  uses: actions/cache@v1.2.1

  with:
- name: Setup Node.js environment

  uses: actions/setup-node@v3.7.0

  with:

    # Set always-auth in npmrc.

    always-auth: # optional, default is false

    # Version Spec of the version to use. Examples: 12.x, 10.15.1, >=10.15.0.

    node-version: # optional

    # File containing the version Spec of the version to use.  Examples: .nvmrc, .node-version, .tool-versions.

    node-version-file: # optional

    # Target architecture for Node to use. Examples: x86, x64. Will use system architecture by default.

    architecture: # optional

    # Set this option if you want the action to check for the latest available version that satisfies the version spec.

    check-latest: # optional

    # Optional registry to set up for auth. Will set the registry in a project level .npmrc and .yarnrc file, and set up auth to read in from env.NODE_AUTH_TOKEN.

    registry-url: # optional

    # Optional scope for authenticating against scoped registries. Will fall back to the repository owner when using the GitHub Packages registry (https://npm.pkg.github.com/).

    scope: # optional

    # Used to pull node distributions from node-versions. Since there's a default, this is typically not supplied by the user. When running this action on github.com, the default value is sufficient. When running on GHES, you can pass a personal access token for github.com if you are experiencing rate limiting.

    token: # optional, default is ${{ github.server_url == 'https://github.com' && github.token || '' }}

    # Used to specify a package manager for caching in the default directory. Supported values: npm, yarn, pnpm.

    cache: # optional

    # Used to specify the path to a dependency file: package-lock.json, yarn.lock, etc. Supports wildcards or a list of file names for caching multiple dependencies.

    cache-dependency-path: # optional
    # A directory to store and save the cache

    path: 

    # An explicit key for restoring and saving the cache

    key: 
- name: Setup .NET Core SDK

  uses: actions/setup-dotnet@v3.2.0

  with:

    # Optional SDK version(s) to use. If not provided, will install global.json version when available. Examples: 2.2.104, 3.1, 3.1.x, 3.x, 6.0.2xx

    dotnet-version: # optional

    # Optional quality of the build. The possible values are: daily, signed, validated, preview, ga.

    dotnet-quality: # optional

    # Optional global.json location, if your global.json isn't located in the root of the repo.

    global-json-file: # optional

    # Optional package source for which to set up authentication. Will consult any existing NuGet.config in the root of the repo and provide a temporary NuGet.config using the NUGET_AUTH_TOKEN environment variable as a ClearTextPassword

    source-url: # optional

    # Optional OWNER for using packages from GitHub Package Registry organizations/users other than the current repository's owner. Only used if a GPR URL is also provided in source-url

    owner: # optional

    # Optional NuGet.config location, if your NuGet.config isn't located in the root of the repo.

    config-file: # optional

    # Optional input to enable caching of the NuGet global-packages folder

    cache: # optional

    # Used to specify the path to a dependency file: packages.lock.json. Supports wildcards or a list of file names for caching multiple dependencies.

    cache-dependency-path: # optional
    
    # An ordered list of keys to use for restoring the cache if no cache hit occurred for key

    restore-keys: # optionalLICENSEhttps://www.youtube.com/embed/VIDEO_ID?playlist=VIDEO_ID&loop=1./mach mochitest --headless devtools/client/<tool>./mach mochitest devtools/client/debugger./mach mochitest -f chrome --tag devtoolsMystorius:gha-enterprise-docs-openid-connect/workspaces/codespaces-blankPOST /{tenant}/oauth2/v2.0/token HTTP/1.1               // Line breaks for clarity

Host: login.microsoftonline.com:443

Content-Type: application/x-www-form-urlencoded

scope=https%3A%2F%2Fgraph.microsoft.com%2F.default

&client_id=97e0a5b7-d745-40b6-94fe-5f77d35c6e05

&client_assertion_type=urn%3Aietf%3Aparams%3Aoauth%3Aclient-assertion-type%3Ajwt-bearer

&client_assertion=eyJhbGciOiJSUzI1NiIsIng1dCI6Imd4OHRHeXN5amNScUtqRlBuZDdSRnd2d1pJMCJ9.eyJ{a lot of characters here}M8U3bSUKKJDEg

&grant_type=client_credentialscurl -X GET -H "Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbG...." 'https://graph.microsoft.com/v1.0/users'GET /v1.0/users HTTP/1.1

Host: graph.microsoft.com:443

Authorization: Bearer eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...AppCreationScripts/AppCreationScripts.mdimport { Octokit } from "octokit";

const octokit = new Octokit({ 

  auth: 'YOUR-TOKEN',

});import { Octokit } from "octokit";

const octokit = new Octokit({ 

  auth: 'YOUR-TOKEN',

});import { Octokit } from "octokit";

const octokit = new Octokit({ 

  auth: process.env.TOKEN,

});import { App } from "octokit";

const app = new App({

  appId: APP_ID,

  privateKey: PRIVATE_KEY,

});

const octokit = await app.getInstallationOctokit(INSTALLATION_ID);import { Octokit } from "octokit";

const octokit = new Octokit({ });await octokit.request("GET /repos/{owner}/{repo}/issues", {

  owner: "github",

  repo: "docs",

  per_page: 2

});import { Octokit } from "octokit";

const octokit = new Octokit({ });await octokit.request("GET /repos/{owner}/{repo}/issues", {

  owner: "github",

  repo: "docs",

  per_page: 2

});await octokit.rest.issues.listForRepo({

  owner: "github",

  repo: "docs",

  per_page: 2

});await octokit.request("POST /markdown/raw", {

  text: "Hello **world**",

  headers: {

    "content-type": "text/plain",

    "x-github-api-version": "2022-11-28",
import { Octokit } from "octokit";

const octokit = new Octokit({ 

  auth: 'YOUR-TOKEN',

});

async function getChangedFiles({owner, repo, pullNumber}) {

  let filesChanged = []

  try {

    const iterator = octokit.paginate.iterator("GET /repos/{owner}/{repo}/pulls/{pull_number}/files", {

      owner: owner,

      repo: repo,

      pull_number: pullNumber,

      per_page: 100,

      headers: {

        "x-github-api-version": "2022-11-28",

      },

    });

    for await (const {data} of iterator) {

      filesChanged = [...filesChanged, ...data.map(fileData => fileData.filename)];

    }

  } catch (error) {

    if (error.response) {

      console.error(`Error! Status: ${error.response.status}. Message: ${error.response.data.message}`)

    }

    console.error(error)

  }

  return filesChanged

}

async function commentIfDataFilesChanged({owner, repo, pullNumber}) {

  const changedFiles = await getChangedFiles({owner, repo, pullNumber});

  const filePathRegex = new RegExp(/\/data\//, "i");

  if (!changedFiles.some(fileName => filePathRegex.test(fileName))) {

    return;

  }

  try {

    const {data: comment} = await octokit.request("POST /repos/{owner}/{repo}/issues/{issue_number}/comments", {

      owner: owner,

      repo: repo,

      issue_number: pullNumber,

      body: `It looks like you changed a data file. These files are auto-generated. \n\nYou must revert any changes to data files before your pull request will be reviewed.`,

      headers: {

        "x-github-api-version": "2022-11-28",

      },

    });

    return comment.html_url;

  } catch (error) {

    if (error.response) {

      console.error(`Error! Status: ${error.response.status}. Message: ${error.response.data.message}`)

    }

    console.error(error)

  }

}

await commentIfDataFilesChanged({owner: "github", repo: "docs", pullNumber: 191});

import { Octokit } from "octokit";

const octokit = new Octokit({ 

  auth: 'YOUR-TOKEN',

});

async function getChangedFiles({owner, repo, pullNumber}) {

  let filesChanged = []

  try {

    const iterator = octokit.paginate.iterator("GET /repos/{owner}/{repo}/pulls/{pull_number}/files", {

      owner: owner,

      repo: repo,

      pull_number: pullNumber,

      per_page: 100,

      headers: {

        "x-github-api-version": "2022-11-28",

      },

    });

    for await (const {data} of iterator) {

      filesChanged = [...filesChanged, ...data.map(fileData => fileData.filename)];

    }

  } catch (error) {

    if (error.response) {

      console.error(`Error! Status: ${error.response.status}. Message: ${error.response.data.message}`)

    }

    console.error(error)

  }

  return filesChanged

}

async function commentIfDataFilesChanged({owner, repo, pullNumber}) {

  const changedFiles = await getChangedFiles({owner, repo, pullNumber});

  const filePathRegex = new RegExp(/\/data\//, "i");

  if (!changedFiles.some(fileName => filePathRegex.test(fileName))) {

    return;

  }

  try {

    const {data: comment} = await octokit.request("POST /repos/{owner}/{repo}/issues/{issue_number}/comments", {

      owner: owner,

      repo: repo,

      issue_number: pullNumber,

      body: `It looks like you changed a data file. These files are auto-generated. \n\nYou must revert any changes to data files before your pull request will be reviewed.`,

      headers: {

        "x-github-api-version": "2022-11-28",

      },

    });

    return comment.html_url;

  } catch (error) {

    if (error.response) {

      console.error(`Error! Status: ${error.response.status}. Message: ${error.response.data.message}`)

    }

    console.error(error)

  }

}

await commentIfDataFilesChanged({owner: "github", repo: "docs", pullNumber: 191});
  },
https://developer.android.com/studio?authuser=0https://github.com/firebase/codelab-friendlychat-android-sL https://firebase.tools | bashecho "# T-tube" >> README.md

git init

git add README.md

git commit -m "first commit"

git branch -M main

git remote add origin https://github.com/Butcher737/T-tube.git

git push -u origin main// When running in debug mode, connect to the Firebase Emulator Suite.

// "10.0.2.2" is a special IP address which allows the Android Emulator

// to connect to "localhost" on the host computer. The port values (9xxx)

// must match the values defined in the firebase.json file.

if (BuildConfig.DEBUG) {

    Firebase.database.useEmulator("10.0.2.2", 9000)

    Firebase.auth.useEmulator("10.0.2.2", 9099)

    Firebase.storage.useEmulator("10.0.2.2", 9199)

}

});- name: Cache

  uses: actions/cache@v1.2.1

  with:
- name: Setup Node.js environment

  uses: actions/setup-node@v3.7.0

  with:

    # Set always-auth in npmrc.

    always-auth: # optional, default is false

    # Version Spec of the version to use. Examples: 12.x, 10.15.1, >=10.15.0.

    node-version: # optional

    # File containing the version Spec of the version to use.  Examples: .nvmrc, .node-version, .tool-versions.

    node-version-file: # optional

    # Target architecture for Node to use. Examples: x86, x64. Will use system architecture by default.

    architecture: # optional

    # Set this option if you want the action to check for the latest available version that satisfies the version spec.

    check-latest: # optional

    # Optional registry to set up for auth. Will set the registry in a project level .npmrc and .yarnrc file, and set up auth to read in from env.NODE_AUTH_TOKEN.

    registry-url: # optional

    # Optional scope for authenticating against scoped registries. Will fall back to the repository owner when using the GitHub Packages registry (https://npm.pkg.github.com/).

    scope: # optional

    # Used to pull node distributions from node-versions. Since there's a default, this is typically not supplied by the user. When running this action on github.com, the default value is sufficient. When running on GHES, you can pass a personal access token for github.com if you are experiencing rate limiting.

    token: # optional, default is ${{ github.server_url == 'https://github.com' && github.token || '' }}

    # Used to specify a package manager for caching in the default directory. Supported values: npm, yarn, pnpm.

    cache: # optional

    # Used to specify the path to a dependency file: package-lock.json, yarn.lock, etc. Supports wildcards or a list of file names for caching multiple dependencies.

    cache-dependency-path: # optional
    # A directory to store and save the cache

    path: 

    # An explicit key for restoring and saving the cache

    key: 
- name: Setup .NET Core SDK

  uses: actions/setup-dotnet@v3.2.0

  with:

    # Optional SDK version(s) to use. If not provided, will install global.json version when available. Examples: 2.2.104, 3.1, 3.1.x, 3.x, 6.0.2xx

    dotnet-version: # optional

    # Optional quality of the build. The possible values are: daily, signed, validated, preview, ga.

    dotnet-quality: # optional

    # Optional global.json location, if your global.json isn't located in the root of the repo.

    global-json-file: # optional

    # Optional package source for which to set up authentication. Will consult any existing NuGet.config in the root of the repo and provide a temporary NuGet.config using the NUGET_AUTH_TOKEN environment variable as a ClearTextPassword

    source-url: # optional

    # Optional OWNER for using packages from GitHub Package Registry organizations/users other than the current repository's owner. Only used if a GPR URL is also provided in source-url

    owner: # optional

    # Optional NuGet.config location, if your NuGet.config isn't located in the root of the repo.

    config-file: # optional

    # Optional input to enable caching of the NuGet global-packages folder

    cache: # optional

    # Used to specify the path to a dependency file: packages.lock.json. Supports wildcards or a list of file names for caching multiple dependencies.

    cache-dependency-path: # optional
    
    # An ordered list of keys to use for restoring the cache if no cache hit occurred for key

    restore-keys: # optional.github/workflows/main.yml
