Test for Snyk
=============

There is a regression introduced in Snyk version 1.1163.0 with regards to [yarn resolutions](https://classic.yarnpkg.com/lang/en/docs/selective-version-resolutions/) - Snyk no longer properly handles projects that use this yarn feature.

[This regression was reported to snyk as request 51461](http://support.snyk.io/hc/requests/51461).

To demonstrate this regression, clone https://github.com/candrews/snyk-workspace-test then run `npx snyk@1.1162.0 test --yarn-workspaces -d` (to see how it used to work correctly) or `npx snyk@1.1163.0 test --yarn-workspaces -d` (to see the new unexpected failure). I suspect this regression was introduced by https://github.com/snyk/cli/commit/97a60a6ca7e0e841f53bd265370cfcfdf3eb56c6.

When using Snyk 1.1162.0, vulnerabilities are reported as expected without error:
```
$ npx snyk@1.1162.0 test --yarn-workspaces -d
Executing: /home/candrews/.npm/_npx/509de7e2271cc761/node_modules/snyk/wrapper_dist/snyk-linux test --yarn-workspaces -d
2023-05-25T18:23:12Z main - Version:               1.1162.0
2023-05-25T18:23:12Z main - Platform:              linux amd64
2023-05-25T18:23:12Z main - API:                   https://api.snyk.io
2023-05-25T18:23:12Z main - Cache:                 /home/candrews/.cache/snyk
2023-05-25T18:23:12Z main - Organization:          759e5516-8d12-451b-bad0-338908f1ee48
2023-05-25T18:23:12Z main - Insecure HTTPS:        false
2023-05-25T18:23:12Z main - Authorization:         56284073aa970f3f994393e5ce079b0a[...]  (type=token)
2023-05-25T18:23:12Z main - Features:              
2023-05-25T18:23:12Z main -   --auth-type=oauth:   Disabled
2023-05-25T18:23:12Z main - Using Legacy CLI to serve the command. (reason: unknown command "test" for "snyk")
2023-05-25T18:23:12Z legacycli:1 - Arguments: [test --yarn-workspaces -d]
2023-05-25T18:23:12Z legacycli:1 - Use StdIO: true
2023-05-25T18:23:12Z legacycli:1 - Init start
2023-05-25T18:23:12Z legacycli:1 - Init-Lock acquired: true (/home/candrews/.cache/snyk/1.1162.0.lock)
2023-05-25T18:23:12Z legacycli:1 - Validating sha256 of /home/candrews/.cache/snyk/1.1162.0/snyk-linux
2023-05-25T18:23:12Z legacycli:1 - expected:  7c57af762b22d5e8ca56ee4f373ca1815c31a0f8714a5da5417c8da316712d77
2023-05-25T18:23:12Z legacycli:1 - actual:    7c57af762b22d5e8ca56ee4f373ca1815c31a0f8714a5da5417c8da316712d77
2023-05-25T18:23:12Z legacycli:1 - Extraction not required
2023-05-25T18:23:12Z legacycli:1 - Init end
2023-05-25T18:23:12Z legacycli:1 - Temporary CertificateLocation: /home/candrews/.cache/snyk/1.1162.0/tmp/snyk-cli-cert-2326241467.crt
2023-05-25T18:23:12Z legacycli:1 - Enabled Proxy Authentication Mechanism: Anyauth
2023-05-25T18:23:12Z legacycli:1 - starting proxy
2023-05-25T18:23:12Z legacycli:1 - Wrapper proxy is listening on port:  46777
2023-05-25T18:23:12Z legacycli:1 - Launching:
2023-05-25T18:23:12Z legacycli:1 - /home/candrews/.cache/snyk/1.1162.0/snyk-linux
2023-05-25T18:23:12Z legacycli:1 - With Arguments:
2023-05-25T18:23:12Z legacycli:1 - test, --yarn-workspaces, -d
2023-05-25T18:23:12Z legacycli:1 - With Environment:
2023-05-25T18:23:12Z legacycli:1 - NODE_EXTRA_CA_CERTS = /home/candrews/.cache/snyk/1.1162.0/tmp/snyk-cli-cert-2326241467.crt
2023-05-25T18:23:12Z legacycli:1 - HTTPS_PROXY = http://snykcli:58307bed-a647-4bb7-9325-ee2f8eab4c34@127.0.0.1:46777
2023-05-25T18:23:12Z legacycli:1 - HTTP_PROXY = http://snykcli:58307bed-a647-4bb7-9325-ee2f8eab4c34@127.0.0.1:46777
2023-05-25T18:23:12Z legacycli:1 - NO_PROXY = localhost,127.0.0.1,::1
2023-05-25T18:23:12Z legacycli:1 - SNYK_SYSTEM_HTTPS_PROXY =
2023-05-25T18:23:12Z legacycli:1 - SNYK_SYSTEM_HTTP_PROXY =
2023-05-25T18:23:12Z legacycli:1 - SNYK_SYSTEM_NO_PROXY =
  snyk test <ref *1> { _: [ [Circular *1] ], debug: true, yarnWorkspaces: true } +0ms
  snyk-test auto detect manifest files, found 2 [
  '/home/candrews/projects/workspace-test/package.json',
  '/home/candrews/projects/workspace-test/workspace1/package.json'
] +0ms
  snyk-yarn-workspaces Processing potential Yarn workspaces (2) +0ms
  snyk-yarn-workspaces Processing /home/candrews/projects/workspace-test as a potential Yarn workspace +0ms
  snyk-yarn-workspaces Processing /home/candrews/projects/workspace-test/workspace1 as a potential Yarn workspace +2ms
  snyk-yarn-workspaces /home/candrews/projects/workspace-test/workspace1/package.json matches an existing workspace pattern +0ms
  snyk-test Found 2 projects from 2 detected manifests +3ms
  snyk Potential policy locations found: [ '/home/candrews/projects/workspace-test' ] +0ms
  snyk:find-files Encountered multiple node lockfiles files, defaulting to /home/candrews/projects/workspace-test/yarn.lock +0ms
  snyk:run-test converting dep-tree to dep-graph { name: 'workspace-test', targetFile: 'package.json' } +0ms
  snyk:run-test done converting dep-tree to dep-graph { uniquePkgsCount: 1 } +1ms
  snyk:prune rootPkg { name: 'workspace-test', version: undefined } +0ms
  snyk:prune prePrunePathsCount: 1 +0ms
  snyk:prune isDenseGraph false +0ms
  snyk Potential policy locations found: [ '/home/candrews/projects/workspace-test/workspace1' ] +19ms
  snyk:find-files Encountered multiple node lockfiles files, defaulting to /home/candrews/projects/workspace-test/yarn.lock +17ms
  snyk:run-test converting dep-tree to dep-graph { name: 'workspace1', targetFile: 'workspace1/package.json' } +16ms
  snyk:run-test done converting dep-tree to dep-graph { uniquePkgsCount: 2 } +1ms
  snyk:prune rootPkg { name: 'workspace1', version: undefined } +17ms
  snyk:prune prePrunePathsCount: 2 +0ms
  snyk:prune isDenseGraph false +0ms
  snyk:run-test sendTestPayload retry step 0 out of 3 +1ms
  snyk sending request to: https://api.snyk.io/v1/test-dep-graph +0ms
  snyk request body size: 534 +0ms
  snyk gzipped request body size: 314 +0ms
  snyk using proxy: http://snykcli:58307bed-a647-4bb7-9325-ee2f8eab4c34@127.0.0.1:46777 +1ms
  snyk:run-test sendTestPayload retry step 0 out of 3 +3ms
  snyk sending request to: https://api.snyk.io/v1/test-dep-graph +2ms
  snyk request body size: 734 +0ms
  snyk gzipped request body size: 373 +0ms
  snyk using proxy: http://snykcli:58307bed-a647-4bb7-9325-ee2f8eab4c34@127.0.0.1:46777 +1ms
2023-05-25T18:23:12Z legacycli:1 - [001] INFO: Running 1 CONNECT handlers
2023-05-25T18:23:12Z legacycli:1 - HandleConnect - basic authentication result:  &{0 <nil> 0x138c620} api.snyk.io:443
2023-05-25T18:23:12Z legacycli:1 - [001] INFO: on 0th handler: &{2 <nil> 0x138c620} api.snyk.io:443
2023-05-25T18:23:12Z legacycli:1 - [002] INFO: Running 1 CONNECT handlers
2023-05-25T18:23:12Z legacycli:1 - HandleConnect - basic authentication result:  &{0 <nil> 0x138c620} api.snyk.io:443
2023-05-25T18:23:12Z legacycli:1 - [002] INFO: on 0th handler: &{2 <nil> 0x138c620} api.snyk.io:443
2023-05-25T18:23:12Z legacycli:1 - [002] INFO: Assuming CONNECT is TLS, mitm proxying it
2023-05-25T18:23:12Z legacycli:1 - [002] INFO: signing for api.snyk.io
2023-05-25T18:23:12Z legacycli:1 - [001] INFO: Assuming CONNECT is TLS, mitm proxying it
2023-05-25T18:23:12Z legacycli:1 - [001] INFO: signing for api.snyk.io
2023-05-25T18:23:12Z legacycli:1 - [003] INFO: req api.snyk.io:443
2023-05-25T18:23:12Z legacycli:1 - [003] INFO: Sending request POST https://api.snyk.io:443/v1/test-dep-graph?org=
2023-05-25T18:23:12Z legacycli:1 - [004] INFO: req api.snyk.io:443
2023-05-25T18:23:12Z legacycli:1 - [004] INFO: Sending request POST https://api.snyk.io:443/v1/test-dep-graph?org=
2023-05-25T18:23:13Z legacycli:1 - [004] INFO: resp 200 OK
2023-05-25T18:23:13Z legacycli:1 - [003] INFO: resp 200 OK
2023-05-25T18:23:13Z legacycli:1 - [001] INFO: Exiting on EOF
2023-05-25T18:23:13Z legacycli:1 - [002] INFO: Exiting on EOF

Testing /home/candrews/projects/workspace-test...

Organization:      candrews
Package manager:   yarn
Target file:       package.json
Project name:      workspace-test
Open source:       no
Project path:      /home/candrews/projects/workspace-test
Licenses:          enabled

✔ Tested /home/candrews/projects/workspace-test for known issues, no vulnerable paths found.

Next steps:
- Run `snyk monitor` to be notified about new related vulnerabilities.
- Run `snyk test` as part of your CI/test.

-------------------------------------------------------

Testing /home/candrews/projects/workspace-test...

Tested 1 dependencies for known issues, found 2 issues, 2 vulnerable paths.


Issues to fix by upgrading:

  Upgrade lodash@4.17.20 to lodash@4.17.21 to fix
  ✗ Regular Expression Denial of Service (ReDoS) [Medium Severity][https://security.snyk.io/vuln/SNYK-JS-LODASH-1018905] in lodash@4.17.20
    introduced by lodash@4.17.20
  ✗ Command Injection [High Severity][https://security.snyk.io/vuln/SNYK-JS-LODASH-1040724] in lodash@4.17.20
    introduced by lodash@4.17.20



Organization:      candrews
Package manager:   yarn
Target file:       workspace1/package.json
Project name:      workspace1
Open source:       no
Project path:      /home/candrews/projects/workspace-test
Licenses:          enabled


Tested 2 projects, 1 contained vulnerable paths.



  snyk Exit code: 1 +0ms
  snyk analytics {
  "args": [
    {
      "debug": true,
      "yarnWorkspaces": true
    }
  ],
  "command": "test",
  "metadata": {
    "upgradable-snyk-protect-paths": 0,
    "local": true,
    "yarnWorkspaces": {
      "scannedProjects": 2,
      "targetFiles": [
        "/home/candrews/projects/workspace-test/package.json",
        "/home/candrews/projects/workspace-test/workspace1/package.json"
      ],
      "packageManagers": [
        "npm",
        "npm"
      ],
      "ignore": [
        "node_modules",
        ".build"
      ]
    },
    "pluginName": "snyk-nodejs-yarn-workspaces",
    "policies": 2,
    "policyLocations": [
      "/home/candrews/projects/workspace-test",
      "/home/candrews/projects/workspace-test/workspace1"
    ],
    "packageManager": [
      "yarn",
      "yarn"
    ],
    "packageName": [
      "workspace-test",
      "workspace1"
    ],
    "packageVersion": "",
    "package": [
      "workspace-test@",
      "workspace1@"
    ],
    "prePrunedPathsCount": 3,
    "depGraph": [
      true,
      true
    ],
    "isDocker": false,
    "vulns-pre-policy": 2,
    "vulns": 2,
    "actionableRemediation": true,
    "error-message": "Vulnerabilities found",
    "error-code": "VULNS",
    "command": "test"
  },
  "os": "Linux 6.3",
  "osPlatform": "linux",
  "osRelease": "6.3.3-200.fc38.x86_64",
  "osArch": "x64",
  "version": "1.1162.0",
  "nodeVersion": "v16.16.0",
  "standalone": true,
  "integrationName": "CLI_V1_PLUGIN",
  "integrationVersion": "1.1162.0",
  "integrationEnvironment": "",
  "integrationEnvironmentVersion": "",
  "id": "be9e8974a843ed09a05bbd08e5ba0544629b36ec",
  "ci": false,
  "durationMs": 1047,
  "metrics": {
    "network_time": {
      "type": "timer",
      "values": [
        387,
        386
      ],
      "total": 773
    },
    "cpu_time": {
      "type": "synthetic",
      "values": [
        274
      ],
      "total": 274
    }
  }
} +0ms
  snyk sending request to: https://api.snyk.io/v1/analytics/cli +681ms
  snyk request body size: 1381 +1ms
  snyk gzipped request body size: 668 +0ms
  snyk using proxy: http://snykcli:58307bed-a647-4bb7-9325-ee2f8eab4c34@127.0.0.1:46777 +0ms
2023-05-25T18:23:13Z legacycli:1 - [005] INFO: Running 1 CONNECT handlers
2023-05-25T18:23:13Z legacycli:1 - HandleConnect - basic authentication result:  &{0 <nil> 0x138c620} api.snyk.io:443
2023-05-25T18:23:13Z legacycli:1 - [005] INFO: on 0th handler: &{2 <nil> 0x138c620} api.snyk.io:443
2023-05-25T18:23:13Z legacycli:1 - [005] INFO: Assuming CONNECT is TLS, mitm proxying it
2023-05-25T18:23:13Z legacycli:1 - [005] INFO: signing for api.snyk.io
2023-05-25T18:23:13Z legacycli:1 - [006] INFO: req api.snyk.io:443
2023-05-25T18:23:13Z legacycli:1 - [006] INFO: Sending request POST https://api.snyk.io:443/v1/analytics/cli
2023-05-25T18:23:13Z legacycli:1 - [006] INFO: resp 200 OK
2023-05-25T18:23:13Z legacycli:1 - [005] INFO: Exiting on EOF
2023-05-25T18:23:13Z legacycli:1 - Proxy successfully shut down
2023-05-25T18:23:13Z legacycli:1 - deleting temp cert file: /home/candrews/.cache/snyk/1.1162.0/tmp/snyk-cli-cert-2326241467.crt
2023-05-25T18:23:13Z legacycli:1 - deleted temp cert file: /home/candrews/.cache/snyk/1.1162.0/tmp/snyk-cli-cert-2326241467.crt
2023-05-25T18:23:13Z main - Exiting with 1
2023-05-25T18:23:13Z main - Sending Analytics
2023-05-25T18:23:13Z main - Analytics successfully send
{
  status: 1,
  signal: null,
  output: [ null, null, null ],
  pid: 40346,
  stdout: null,
  stderr: null
}
[1]
```

When using Snyk 1.1163.0, the command fails with a "Your package.json and yarn.lock are probably out of sync." error which should not happen:
```
$ npx snyk@1.1163.0 test --yarn-workspaces -d
Executing: /home/candrews/.npm/_npx/7493b70997bef9e5/node_modules/snyk/wrapper_dist/snyk-linux test --yarn-workspaces -d
2023-05-25T18:23:42Z main - Version:               1.1163.0
2023-05-25T18:23:42Z main - Platform:              linux amd64
2023-05-25T18:23:42Z main - API:                   https://api.snyk.io
2023-05-25T18:23:42Z main - Cache:                 /home/candrews/.cache/snyk
2023-05-25T18:23:42Z main - Organization:          759e5516-8d12-451b-bad0-338908f1ee48
2023-05-25T18:23:42Z main - Insecure HTTPS:        false
2023-05-25T18:23:42Z main - Authorization:         56284073aa970f3f994393e5ce079b0a[...]  (type=token)
2023-05-25T18:23:42Z main - Features:              
2023-05-25T18:23:42Z main -   --auth-type=oauth:   Disabled
2023-05-25T18:23:42Z main - Using Legacy CLI to serve the command. (reason: unknown command "test" for "snyk")
2023-05-25T18:23:42Z legacycli:1 - Arguments: [test --yarn-workspaces -d]
2023-05-25T18:23:42Z legacycli:1 - Use StdIO: true
2023-05-25T18:23:42Z legacycli:1 - Init start
2023-05-25T18:23:42Z legacycli:1 - Init-Lock acquired: true (/home/candrews/.cache/snyk/1.1163.0.lock)
2023-05-25T18:23:42Z legacycli:1 - Validating sha256 of /home/candrews/.cache/snyk/1.1163.0/snyk-linux
2023-05-25T18:23:42Z legacycli:1 - open /home/candrews/.cache/snyk/1.1163.0/snyk-linux: no such file or directory
2023-05-25T18:23:42Z legacycli:1 - Extract cliv1 to /home/candrews/.cache/snyk/1.1163.0/snyk-linux
2023-05-25T18:23:42Z legacycli:1 - Validating sha256 of /home/candrews/.cache/snyk/1.1163.0/snyk-linux
2023-05-25T18:23:42Z legacycli:1 - expected:  c180466f0b2f2bf7be095b0ffe36553a31ad4c720bbf3dcfe9e3d62703dc2da5
2023-05-25T18:23:42Z legacycli:1 - actual:    c180466f0b2f2bf7be095b0ffe36553a31ad4c720bbf3dcfe9e3d62703dc2da5
2023-05-25T18:23:42Z legacycli:1 - Extracted cliv1 successfully
2023-05-25T18:23:42Z legacycli:1 - Init end
2023-05-25T18:23:42Z legacycli:1 - Temporary CertificateLocation: /home/candrews/.cache/snyk/1.1163.0/tmp/snyk-cli-cert-1340978639.crt
2023-05-25T18:23:42Z legacycli:1 - Enabled Proxy Authentication Mechanism: Anyauth
2023-05-25T18:23:42Z legacycli:1 - starting proxy
2023-05-25T18:23:42Z legacycli:1 - Wrapper proxy is listening on port:  33389
2023-05-25T18:23:42Z legacycli:1 - Launching:
2023-05-25T18:23:42Z legacycli:1 - /home/candrews/.cache/snyk/1.1163.0/snyk-linux
2023-05-25T18:23:42Z legacycli:1 - With Arguments:
2023-05-25T18:23:42Z legacycli:1 - test, --yarn-workspaces, -d
2023-05-25T18:23:42Z legacycli:1 - With Environment:
2023-05-25T18:23:42Z legacycli:1 - NODE_EXTRA_CA_CERTS = /home/candrews/.cache/snyk/1.1163.0/tmp/snyk-cli-cert-1340978639.crt
2023-05-25T18:23:42Z legacycli:1 - HTTPS_PROXY = http://snykcli:fab57547-1fac-4c82-9131-fa8a672b4c63@127.0.0.1:33389
2023-05-25T18:23:42Z legacycli:1 - HTTP_PROXY = http://snykcli:fab57547-1fac-4c82-9131-fa8a672b4c63@127.0.0.1:33389
2023-05-25T18:23:42Z legacycli:1 - NO_PROXY = localhost,127.0.0.1,::1
2023-05-25T18:23:42Z legacycli:1 - SNYK_SYSTEM_HTTPS_PROXY =
2023-05-25T18:23:42Z legacycli:1 - SNYK_SYSTEM_HTTP_PROXY =
2023-05-25T18:23:42Z legacycli:1 - SNYK_SYSTEM_NO_PROXY =
  snyk test <ref *1> { _: [ [Circular *1] ], debug: true, yarnWorkspaces: true } +0ms
  snyk-test auto detect manifest files, found 2 [
  '/home/candrews/projects/workspace-test/package.json',
  '/home/candrews/projects/workspace-test/workspace1/package.json'
] +0ms
  snyk-yarn-workspaces Processing potential Yarn workspaces (2) +0ms
  snyk-yarn-workspaces Processing /home/candrews/projects/workspace-test as a potential Yarn workspace +0ms
  snyk-yarn-workspaces Processing /home/candrews/projects/workspace-test/workspace1 as a potential Yarn workspace +2ms
  snyk-yarn-workspaces /home/candrews/projects/workspace-test/workspace1/package.json matches an existing workspace pattern +0ms
  snyk:run-test Error running test {
  error: OutOfSyncError: Dependency lodash@^4.17.21 was not found in yarn.lock. Your package.json and yarn.lock are probably out of sync. Please run "yarn install" and try again.
      at getYarnLockV2ChildNode (/snapshot/snyk/dist/cli/webpack:/snyk/node_modules/snyk-nodejs-lockfile-parser/dist/dep-graph-builders/yarn-lock-v2/utils.js:110:1)
      at dfsVisit (/snapshot/snyk/dist/cli/webpack:/snyk/node_modules/snyk-nodejs-lockfile-parser/dist/dep-graph-builders/yarn-lock-v2/build-depgraph-simple.js:38:1)
      at buildDepGraphYarnLockV2Simple (/snapshot/snyk/dist/cli/webpack:/snyk/node_modules/snyk-nodejs-lockfile-parser/dist/dep-graph-builders/yarn-lock-v2/build-depgraph-simple.js:22:1)
      at Object.parseYarnLockV2Project (/snapshot/snyk/dist/cli/webpack:/snyk/node_modules/snyk-nodejs-lockfile-parser/dist/dep-graph-builders/yarn-lock-v2/simple.js:12:1)
      at Object.processYarnWorkspaces [as handler] (/snapshot/snyk/dist/cli/webpack:/snyk/src/lib/plugins/nodejs-plugin/yarn-workspaces-parser.ts:121:17)
      at Object.getDepsFromPlugin (/snapshot/snyk/dist/cli/webpack:/snyk/src/lib/plugins/get-deps-from-plugin.ts:62:24)
      at assembleLocalPayloads (/snapshot/snyk/dist/cli/webpack:/snyk/src/lib/snyk-test/run-test.ts:595:18)
      at runTest (/snapshot/snyk/dist/cli/webpack:/snyk/src/lib/snyk-test/run-test.ts:338:22)
      at test (/snapshot/snyk/dist/cli/webpack:/snyk/src/cli/commands/test/index.ts:155:13)
      at runCommand (/snapshot/snyk/dist/cli/webpack:/snyk/src/cli/main.ts:51:25)
      at main (/snapshot/snyk/dist/cli/webpack:/snyk/src/cli/main.ts:299:11)
      at /snapshot/snyk/dist/cli/webpack:/snyk/src/cli/index.ts:13:3
      at Object.callHandlingUnexpectedErrors (/snapshot/snyk/dist/cli/webpack:/snyk/src/lib/unexpected-error.ts:28:5) {
    code: 422,
    dependencyName: 'lodash@^4.17.21',
    lockFileType: 'yarn2'
  }
} +0ms
  snyk-test Failed to test 1 projects, errors: +0ms
  snyk-test error: FailedToRunTestError: Dependency lodash@^4.17.21 was not found in yarn.lock. Your package.json and yarn.lock are probably out of sync. Please run "yarn install" and try again.
    at runTest (/snapshot/snyk/dist/cli/webpack:/snyk/src/lib/snyk-test/run-test.ts:375:11)
    at test (/snapshot/snyk/dist/cli/webpack:/snyk/src/cli/commands/test/index.ts:155:13)
    at runCommand (/snapshot/snyk/dist/cli/webpack:/snyk/src/cli/main.ts:51:25)
    at main (/snapshot/snyk/dist/cli/webpack:/snyk/src/cli/main.ts:299:11)
    at /snapshot/snyk/dist/cli/webpack:/snyk/src/cli/index.ts:13:3
    at Object.callHandlingUnexpectedErrors (/snapshot/snyk/dist/cli/webpack:/snyk/src/lib/unexpected-error.ts:28:5) +0ms
Error: 
Testing /home/candrews/projects/workspace-test...

Dependency lodash@^4.17.21 was not found in yarn.lock. Your package.json and yarn.lock are probably out of sync. Please run "yarn install" and try again.
    at test (/snapshot/snyk/dist/cli/webpack:/snyk/src/cli/commands/test/index.ts:286:19)
    at runCommand (/snapshot/snyk/dist/cli/webpack:/snyk/src/cli/main.ts:51:25)
    at main (/snapshot/snyk/dist/cli/webpack:/snyk/src/cli/main.ts:299:11)
    at /snapshot/snyk/dist/cli/webpack:/snyk/src/cli/index.ts:13:3
    at Object.callHandlingUnexpectedErrors (/snapshot/snyk/dist/cli/webpack:/snyk/src/lib/unexpected-error.ts:28:5)
  snyk Exit code: 2 +0ms
  snyk analytics {
  "args": [
    {
      "debug": true,
      "yarnWorkspaces": true
    }
  ],
  "command": "bad-command",
  "metadata": {
    "upgradable-snyk-protect-paths": 0,
    "local": true,
    "error-message": "\nTesting /home/candrews/projects/workspace-test...\n\nDependency lodash@^4.17.21 was not found in yarn.lock. Your package.json and yarn.lock are probably out of sync. Please run \"yarn install\" and try again.",
    "error": "Error: \nTesting /home/candrews/projects/workspace-test...\n\nDependency lodash@^4.17.21 was not found in yarn.lock. Your package.json and yarn.lock are probably out of sync. Please run \"yarn install\" and try again.\n    at test (/snapshot/snyk/dist/cli/webpack:/snyk/src/cli/commands/test/index.ts:286:19)\n    at runCommand (/snapshot/snyk/dist/cli/webpack:/snyk/src/cli/main.ts:51:25)\n    at main (/snapshot/snyk/dist/cli/webpack:/snyk/src/cli/main.ts:299:11)\n    at /snapshot/snyk/dist/cli/webpack:/snyk/src/cli/index.ts:13:3\n    at Object.callHandlingUnexpectedErrors (/snapshot/snyk/dist/cli/webpack:/snyk/src/lib/unexpected-error.ts:28:5)",
    "error-code": 422,
    "command": "test"
  },
  "os": "Linux 6.3",
  "osPlatform": "linux",
  "osRelease": "6.3.3-200.fc38.x86_64",
  "osArch": "x64",
  "version": "1.1163.0",
  "nodeVersion": "v16.16.0",
  "standalone": true,
  "integrationName": "CLI_V1_PLUGIN",
  "integrationVersion": "1.1163.0",
  "integrationEnvironment": "",
  "integrationEnvironmentVersion": "",
  "id": "bbfe68cc9969c106caad9e2b8535e583711fe296",
  "ci": false,
  "durationMs": 932,
  "metrics": {
    "network_time": {
      "type": "timer",
      "values": [],
      "total": 0
    },
    "cpu_time": {
      "type": "synthetic",
      "values": [
        932
      ],
      "total": 932
    }
  }
} +0ms
  snyk sending request to: https://api.snyk.io/v1/analytics/cli +0ms
  snyk request body size: 1547 +1ms
  snyk gzipped request body size: 697 +0ms
  snyk using proxy: http://snykcli:fab57547-1fac-4c82-9131-fa8a672b4c63@127.0.0.1:33389 +0ms
2023-05-25T18:23:43Z legacycli:1 - [001] INFO: Running 1 CONNECT handlers
2023-05-25T18:23:43Z legacycli:1 - HandleConnect - basic authentication result:  &{0 <nil> 0x138c620} api.snyk.io:443
2023-05-25T18:23:43Z legacycli:1 - [001] INFO: on 0th handler: &{2 <nil> 0x138c620} api.snyk.io:443
2023-05-25T18:23:43Z legacycli:1 - [001] INFO: Assuming CONNECT is TLS, mitm proxying it
2023-05-25T18:23:43Z legacycli:1 - [001] INFO: signing for api.snyk.io
2023-05-25T18:23:43Z legacycli:1 - [002] INFO: req api.snyk.io:443
2023-05-25T18:23:43Z legacycli:1 - [002] INFO: Sending request POST https://api.snyk.io:443/v1/analytics/cli
2023-05-25T18:23:43Z legacycli:1 - [002] INFO: resp 200 OK
2023-05-25T18:23:43Z legacycli:1 - [001] INFO: Exiting on EOF
2023-05-25T18:23:43Z legacycli:1 - Proxy successfully shut down
2023-05-25T18:23:43Z legacycli:1 - deleting temp cert file: /home/candrews/.cache/snyk/1.1163.0/tmp/snyk-cli-cert-1340978639.crt
2023-05-25T18:23:43Z legacycli:1 - deleted temp cert file: /home/candrews/.cache/snyk/1.1163.0/tmp/snyk-cli-cert-1340978639.crt
2023-05-25T18:23:43Z main - Exiting with 2
2023-05-25T18:23:43Z main - Sending Analytics
2023-05-25T18:23:44Z main - Analytics successfully send
{
  status: 2,
  signal: null,
  output: [ null, null, null ],
  pid: 40443,
  stdout: null,
  stderr: null
}
[2]
```