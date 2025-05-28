# Step-by-step: Demonstrate a "Hello World" Git pre-commit hook

1. Open a terminal and go to the `24._Git_Hooks` folder:
```sh
cd "24._Git_Hooks/"
```

2. Make sure the [`.git/hooks/pre-commit`](.git\hooks\pre-commit) file already contains:
```sh
#!/bin/sh
echo "Hello, world!"
```

3. Make a change to any file in the `24._Git_Hooks` folder, then stage the change:
```sh
git add .
```

4. Commit the change:
```sh
git commit -m "Test pre-commit hook"
```

You should see `"Hello, world!"` printed in the terminal before the commit completes.