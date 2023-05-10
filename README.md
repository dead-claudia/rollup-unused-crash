# Crash cases

Run `npm install` before any of this. It's all using commands, but the behavior is the same with config files.

- [Invalid to invalid](#invalid-to-invalid)
  - [Inline config](#inline-config)
  - [Project](#project)
- [Invalid to valid](#invalid-to-valid)
  - [Inline config](#inline-config-1)
  - [Project](#project-1)
- [Valid to invalid](#valid-to-invalid)
  - [Inline config](#inline-config-2)
  - [Project](#project-2)

## Invalid to invalid

### Inline config

1. Ensure the first line of `index.ts` is uncommented and no `dist.js` file exists.

2. Start the watcher server. This must run concurrently to all the subsequent steps.

    ```sh
    ./node_modules/.bin/rollup -w -i index.ts -o dist.js -p '@rollup/plugin-typescript={noEmitOnError:true,noUnusedLocals:true}'
    ```

    It will error, but this is expected.

3. Save `index.ts` without modification to update the access time and trigger the watcher.

    ```sh
    touch index.ts
    ```

Node v16.20.0 will silently exit with code 0. Node v20.1.0 reports this error:

```
node:internal/errors:490
    ErrorCaptureStackTrace(err);
    ^

TypeError [ERR_INVALID_ARG_TYPE]: The "code" argument must be of type number. Received an instance of Error
    at process.set [as exitCode] (node:internal/bootstrap/node:124:9)
    at process.exit (node:internal/process/per_thread:188:24)
    at process.close (/home/claudia/rollup-typescript-silent-crash/node_modules/rollup/dist/shared/watch-cli.js:543:19) {
  code: 'ERR_INVALID_ARG_TYPE'
}
```

### Project

1. Ensure the first line of `index.ts` is uncommented and no `dist.js` file exists.

2. Start the watcher server. This must run concurrently to all the subsequent steps.

    ```sh
    ./node_modules/.bin/rollup -w -i index.ts -o dist.js -p '@rollup/plugin-typescript={noEmitOnError:true,project:"tsconfig.json"}'
    ```

    It will error, but this is expected.

3. Save `index.ts` without modification to update the access time and trigger the watcher.

    ```sh
    touch index.ts
    ```

Node v16.20.0 will silently exit with code 0. Node v20.1.0 reports this error:

```
node:internal/errors:490
    ErrorCaptureStackTrace(err);
    ^

TypeError [ERR_INVALID_ARG_TYPE]: The "code" argument must be of type number. Received an instance of Error
    at process.set [as exitCode] (node:internal/bootstrap/node:124:9)
    at process.exit (node:internal/process/per_thread:188:24)
    at process.close (/home/claudia/rollup-typescript-silent-crash/node_modules/rollup/dist/shared/watch-cli.js:543:19) {
  code: 'ERR_INVALID_ARG_TYPE'
}
```

## Invalid to valid

### Inline config

1. Ensure the first line of `index.ts` is uncommented and no `dist.js` file exists.

2. Start the watcher server. This must run concurrently to all the subsequent steps.

    ```sh
    ./node_modules/.bin/rollup -w -i index.ts -o dist.js -p '@rollup/plugin-typescript={noEmitOnError:true,noUnusedLocals:true}'
    ```

    It will error, but this is expected.

3. Comment out the first line of `index.ts` and save.

    This will silently fail to update on both Node versions.

4. Save `index.ts` a second time, this time without modification, to update the access time and trigger the watcher.

    ```sh
    touch index.ts
    ```

    This will silently fail to update on both Node versions.

### Project

1. Ensure the first line of `index.ts` is uncommented and no `dist.js` file exists.

2. Start the watcher server. This must run concurrently to all the subsequent steps.

    ```sh
    ./node_modules/.bin/rollup -w -i index.ts -o dist.js -p '@rollup/plugin-typescript={noEmitOnError:true,project:"tsconfig.json"}'
    ```

    It will error, but this is expected.

3. Comment out the first line of `index.ts` and save.

    This will silently fail to update on both Node versions.

4. Save `index.ts` a second time, this time without modification, to update the access time and trigger the watcher.

    ```sh
    touch index.ts
    ```

    This will silently fail to update on both Node versions.

## Valid to invalid

### Inline config

1. Ensure the first line of `index.ts` is commented out.

2. Start the watcher server. This must run concurrently to all the subsequent steps.

    ```sh
    ./node_modules/.bin/rollup -w -i index.ts -o dist.js -p '@rollup/plugin-typescript={noEmitOnError:true,noUnusedLocals:true}'
    ```

    It will correctly generate `dist.js`.

3. Delete `dist.js`, so its regeneration can be observed.

4. Uncomment the first line of `index.ts` and save.

First, it'll refresh the screen with the following message:

```
rollup v3.21.6
bundles index.ts → dist.js...
```

Then, Node v16.20.0 will silently exit with code 0. Node v20.1.0 reports this error instead:

```
node:internal/errors:490
    ErrorCaptureStackTrace(err);
    ^

TypeError [ERR_INVALID_ARG_TYPE]: The "code" argument must be of type number. Received an instance of Error
    at process.set [as exitCode] (node:internal/bootstrap/node:124:9)
    at process.exit (node:internal/process/per_thread:188:24)
    at process.close (/home/claudia/rollup-typescript-silent-crash/node_modules/rollup/dist/shared/watch-cli.js:543:19) {
  code: 'ERR_INVALID_ARG_TYPE'
}
```

### Project

1. Ensure the first line of `index.ts` is commented out and no `dist.js` file exists.

2. Start the watcher server. This must run concurrently to all the subsequent steps.

    ```sh
    ./node_modules/.bin/rollup -w -i index.ts -o dist.js -p '@rollup/plugin-typescript={noEmitOnError:true,project:"tsconfig.json"}'
    ```

    It will correctly generate `dist.js`.

3. Delete `dist.js`, so its regeneration can be observed.

4. Uncomment the first line of `index.ts` and save.

First, it'll refresh the screen with the following message:

```
rollup v3.21.6
bundles index.ts → dist.js...
```

Then, Node v16.20.0 will silently exit with code 0. Node v20.1.0 reports this error instead:

```
node:internal/errors:490
    ErrorCaptureStackTrace(err);
    ^

TypeError [ERR_INVALID_ARG_TYPE]: The "code" argument must be of type number. Received an instance of Error
    at process.set [as exitCode] (node:internal/bootstrap/node:124:9)
    at process.exit (node:internal/process/per_thread:188:24)
    at process.close (/home/claudia/rollup-typescript-silent-crash/node_modules/rollup/dist/shared/watch-cli.js:543:19) {
  code: 'ERR_INVALID_ARG_TYPE'
}
```
