1. ` git show aefea `  видим полный хеш и комментарий коммита.

2. ` git show --pretty 85024d3 ` можем увидеть тег в скобках справа от хеша коммита.

3. ` git show b8d720^ ` видим, что родителем коммита b8d720 является коммит 56cd7859e05c36c06b56d013b55a252d0bb7e158, который образовался в результате мерджа коммитов 58dcac4b7 и ffbcf5581

4. ```
b14b74c4939dcab573326f4e3ee2a62e23e12f89 [Website] vmc provider links
3f235065b9347a758efadc92295b540ee0a5e26e Update CHANGELOG.md
6ae64e247b332925b872447e9ce869657281c2bf registry: Fix panic when server is unreachable
5c619ca1baf2e21a155fcdb4c264cc9e24a2a353 website: Remove links to the getting started guide's old location
06275647e2b53d97d4f0a19a0fec11f6d69820b5 Update CHANGELOG.md
d5f9411f5108260320064349b757f55c09bc4b80 command: Fix bug when using terraform login on Windows
4b6d06cc5dcb78af637bbb19c198faff37a066ed Update CHANGELOG.md
dd01a35078f040ca984cdd349f18d0b67e486c35 Update CHANGELOG.md
225466bc3e5f35baa5d07197bbc079345b77525e Cleanup after v0.12.23 release
```

5. Коммит был найден в ходе выполнения следующих команд: 
` git log -S "func providerSource" --oneline `
` git show 5af1e6234 `
Коммит, в котором была создана функция func providerSource - ` 5af1e6234 `

6. ` git log -S "globalPluginDirs" --oneline `
```
35a058fb3 main: configure credentials from the CLI config file
c0b176109 prevent log output during init
8364383c3 Push plugin discovery down into command package
```

7. ` git log -S "synchronizedWriters" `
```
commit 5ac311e2a91e381e2f52234668b49ba670aa0fe5
Author: Martin Atkins <mart@degeneration.co.uk>
```


