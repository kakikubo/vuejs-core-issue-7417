# vuejs-core-issue-7417

This is the repository to reproduce the 3.2.45 [issue](https://github.com/vuejs/core/issues/7417).

## Overview

Start vue (up to 3.2.44) with yarn dev and access it with a browser using the following sample code.

```vue
<script setup lang="ts">
import { ref } from "vue";
const now = new Date();
const nowStr = now.toLocaleTimeString();
let timeStr = nowStr;
const timeStrRef = ref(nowStr);
function changeTime(): void {
  const newTime = new Date();
  const newTimeStr = newTime.toLocaleTimeString();
  timeStr = newTimeStr;
  timeStrRef.value = newTimeStr;
}
setInterval(changeTime, 1000);
</script>

<template>
  <p>timeStr: {{ timeStr }}</p>
  <p>timeStr(ref): {{ timeStrRef }}</p>
</template>
```

### What is expected?

It can be reproduced as follows

```bash
% cd 3.2.44
% yarn
% yarn dev --open
```

* timeStr => timeStr is fixed at the time when the screen is opened in the browser
* timeStr(ref) => timeStr(ref) is updated every second since the screen was opened in the browser


### What is actually happening?

It can be reproduced as follows

```bash
% cd 3.2.45
% yarn
% yarn dev --open
```

When the previous code is executed in vue 3.2.45, the behavior of timeStr and timeStr(ref) is the same.
* timeStr => timeStr is updated every second since the screen was opened in the browser
* timeStr(ref) => timeStr(ref) is updated every second since the screen was opened in the browser

### System Info

```shell
System:
    OS: macOS 12.6.2
    CPU: (10) arm64 Apple M1 Pro
    Memory: 114.20 MB / 32.00 GB
    Shell: 5.8.1 - /bin/zsh
  Binaries:
    Node: 18.12.1 - ~/.asdf/installs/nodejs/18.12.1/bin/node
    Yarn: 1.22.19 - ~/brew/bin/yarn
    npm: 8.19.2 - ~/.asdf/plugins/nodejs/shims/npm
  Browsers:
    Chrome: 108.0.5359.124
    Safari: 16.2
  npmPackages:
    vue: 3.2.45 => 3.2.45
```


### Any additional comments?

Is there a mistake in the way it is written? If so, I would like to know if you can help me.

**note**

I can only reproduce this event with Vue SFC PlayGround with the same behavior as 3.2.45. You can actually use the terminal to specify 3.2.44 or 3.2.45 in package.json and run rm yarn.lock && yarn install && yarn dev, etc. to confirm.

This problem is **not reproduced** by SFC playground.
