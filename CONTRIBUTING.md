# Guide de contribution à Vite

Nous sommes très heureux que vous souhaitiez contribuer à Vite ! Avant de soumettre votre contribution, veuillez lire le guide suivant. Nous vous suggérons également de lire la [Philosophie du projet](https://fr.vitejs.dev/guide/philosophy) dans notre documentation.

Vous pouvez utiliser [StackBlitz Codeflow](https://stackblitz.com/codeflow) pour corriger les bugs ou implémenter des fonctionnalités. Vous verrez un bouton Codeflow sur les bugs pour commencer une PR pour les corriger. Un bouton apparaîtra également sur les PR pour les revoir sans avoir besoin de vérifier la branche localement. Lorsque vous utilisez Codeflow, le dépôt Vite sera cloné pour vous dans un éditeur en ligne, avec le package Vite build en mode observateur (watch) prêt à tester vos modifications. Si vous souhaitez en savoir plus, consultez la [documentation Codeflow](https://developer.stackblitz.com/codeflow/what-is-codeflow).


[![Ouvrir dans Codeflow](https://developer.stackblitz.com/img/open_in_codeflow.svg)](https://pr.new/vitejs/vite)

## Configuration du dépôt

Pour développer en local, forkez le dépôt Vite et clonez-le sur votre machine locale. Le dépôt Vite est un monorepo utilisant pnpm workspaces. Le gestionnaire de paquets utilisé pour installer et lier les dépendances doit être [pnpm](https://pnpm.io/).


Pour développer et tester le package du noyau `vite`:

1. Exécutez `pnpm i` dans le répertoire racine de Vite.

2. Lancer `pnpm run build` dans le répertoire racine de Vite.

3. Si vous développez sur Vite en lui-même, vous pouvez aller dans `packages/vite` et lancer `pnpm run dev` pour re-build Vite automatiquement chaque fois que vous changez son code.

Vous pouvez également utiliser [Vite.js Docker Dev](https://github.com/nystudio107/vitejs-docker-dev) pour disposer d'un container Docker configuré pour le développement de Vite.

> Vite utilise pnpm v8. Si vous travaillez sur plusieurs projets avec différentes versions de pnpm, il est recommandé d’activer [Corepack](https://github.com/nodejs/corepack) en lançant `corepack enable`.

### Ignorer les commits lors de l’exécution de `git blame`

Nous avons un fichier `.git-blame-ignore-revs` pour ignorer les modifications de formatage.
Pour que ce fichier soit utilisé par`git blame`, vous devez lancer la commande suivante.

```sh
git config --local blame.ignoreRevsFile .git-blame-ignore-revs
```

## Débogage

Pour utiliser les points d’arrêt et explorer l’exécution du code, vous pouvez utiliser la fonctionnalité [« Run and Debug »](https://code.visualstudio.com/docs/editor/debugging) de VS Code.

1. Ajoutez une instruction `debugger` dans le hook `afterAll` de `playground/vitestSetup.ts`. Cela mettra en pause l’exécution avant que les tests ne quittent et l’instance de navigateur Playwright ne quitte.

2. Cliquez sur l’icône « Run and Debug » dans la barre d’activité de l’éditeur, qui ouvre la vue [_Run and Debug_](https://code.visualstudio.com/docs/editor/debugging#_run-and-debug-view).

3. Cliquez sur le bouton « JavaScript Debug Terminal » dans la vue [_Run and Debug_](https://code.visualstudio.com/docs/editor/debugging#_run-and-debug-view), qui ouvre un terminal dans VS Code.

4. Depuis ce terminal, allez dans`playground/xxx` et lancez`pnpm run dev`.

5. L’exécution s’arrêtera à l’instruction`debugger` et vous pouvez utiliser la [Barre de débogage](https://code.visualstudio.com/docs/editor/debugging#_debug-actions) pour continuer, passez à l’étape suivante et redémarrez le processus…

### Débogage des erreurs dans les tests Vitest, en utilisant Playwright (Chromium)

Certaines erreurs sont masquées et cachées en raison des couches d'abstraction et de la nature sandboxée rajoutée par Vitest, Playwright et Chromium. Afin de voir, dans ces instances, ce qui se échoue réellement et le contenu de la console de devtools, suivez cette configuration :

1. Ajoutez une instruction `debugger` dans le hook `afterAll` de `playground/vitestSetup.ts`. Cela mettra en pause l’exécution avant que les tests ne se termine et que l’instance de navigateur Playwright ne se ferme.

2. Lancez les tests avec la commande de script `debug-serve`, qui activera le débogage distant : `pnpm run debug-serve resolve`.

3. Attendez que le devtools de l’inspecteur s’ouvre dans votre navigateur et que le débogage soit attaché.

4. Dans le panneau des sources dans la colonne de droite, cliquez sur le bouton de lecture pour reprendre l’exécution et permettre aux tests de s’exécuter, ce qui ouvrira une instance de Chromium.

5. En sélectionnant l’instance de Chromium, vous pouvez ouvrir les outils de développement du navigateur et inspecter la console pour trouver les problèmes sous-jacents.

6. Pour tout fermer, arrêtez simplement le processus de test dans votre terminal.

## Test de Vite avec des packages externes

You may wish to test your locally modified copy of Vite against another package that is built with Vite. For pnpm, after building Vite, you can use [`pnpm.overrides`](https://pnpm.io/package_json#pnpmoverrides) to do this. Note that `pnpm.overrides` must be specified in the root `package.json`, and you must list the package as a dependency in the root `package.json`:

Vous pourriez souhaiterez de tester votre copie modifiée de Vite sun un autre package buildé avec Vite. Avec pnpm, après avoir build Vite, vous pouvez utiliser [`pnpm.overrides`](https://pnpm.io/package_json#pnpmoverrides) pour cela. Notez que `pnpm.overrides` doit être spécifié dans la `package.json` à la racine et vous devez y lister le package comme une dépendance dans ce `package.json` :


```json
{
  "dependencies": {
    "vite": "^4.0.0"
  },
  "pnpm": {
    "overrides": {
      "vite": "link:../path/to/vite/packages/vite"
    }
  }
}
```

Puis, relancez `pnpm install` pour lier ce package.

## Exécution des tests

### Tests d’intégration

Chaque package sous `playground/` contient un répertoire `__tests__`. Les tests sont exécutés en utilisant [Vitest](https://vitest.dev/) + [Playwright](https://playwright.dev/) avec des intégrations personnalisées pour rendre l’écriture de tests simple. La configuration détaillée se trouve à l’intérieur de `vitest.config.e2e.js`et des fichiers`playground/vitest*`.

Certains playgrounds définissent des variantes pour exécuter la même application avec différentes configurations. Par convention, lorsque vous exécutez un fichier de spécification de test, dans un répertoire imbriqué dans `__tests__`, le configuration tentera d’utiliser un fichier nommé `vite.config-{folderName}.js` situé à la racine du playground. Vous pouvez voir un exemple de variantes dans le [playground des assets](https://github.com/vitejs/vite/tree/main/playground/assets).

Avant d’exécuter les tests, assurez-vous que [Vite a bien été build](#repo-setup). Sur Windows, vous souhaiteriez peut-être [activer le mode Développeur](https://docs.microsoft.com/en-us/windows/apps/get-started/enable-your-device-for-development) pour résoudre [les problèmes avec la création de liens symboliques pour les non-administrateurs](https://github.com/vitejs/vite/issues/7390). De plus, vous souhaiteriez peut-être [définir `core.symlinks` à `true` pour résoudre les problèmes avec les liens symboliques dans git](https://github.com/vitejs/vite/issues/5242).

Chaque test d’intégration peut être exécuté soit en mode serveur, soit en mode build.

- `pnpm test` par défaut, exécute tous les tests d’intégration dans les modes serveur et build, ainsi que les tests unitaires.

- `pnpm run test-serve` exécute les tests uniquement en mode serveur.

- `pnpm run test-build` exécute les tests uniquement en mode build.

- `pnpm run test-serve [match]` ou `pnpm run test-build [match]` exécute les tests dans des packages spécifiques qui correspondent au filtre donné. Par exemple, `pnpm run test-serve asset` exécute les tests pour les deux `playground/asset` et `vite/src/node/__tests__/asset` en le mode serveur.

Note : Le filtrage des packages n’est pas disponible pour le script `pnpm test`, qui exécute tous les tests.

### Tests unitaires

Outre les tests sous `playground/` pour les tests d’intégration, les packages peuvent contenir des tests unitaires sous leur répertoire `__tests__`. Les tests unitaires sont alimentés par [Vitest](https://vitest.dev/). La configuration détaillée est à l’intérieur des fichiers`vitest.config.ts`.

- `pnpm run test-unit` exécute les tests unitaires de chaque package.

- `pnpm run test-unit [match]` exécute les tests dans des packages spécifiques qui correspondent au filtre donné.

### Environnement de test et helpers

Dans les tests sous playground, vous pouvez importer l’objet `page` depuis `~utils`, qui est une instance de [`Page`](https://playwright.dev/docs/api/class-page) de Playwright ayant déjà navigué vers la page servie du playground actuel. Donc, écrire un test est aussi simple que :


```js
import { page } from '~utils'

test('should work', async () => {
  expect(await page.textContent('.foo')).toMatch('foo')
})
```
Certains helpers communs (e.g.`testDir`, `isBuild`, ou `editFile`) sont également disponibles dans les utils. Le code source est situé dans `playground/test-utils.ts`.

Note: The test build environment uses a [different default set of Vite config](https://github.com/vitejs/vite/blob/main/playground/vitestSetup.ts#L102-L122) to skip transpilation during tests to make it faster. This may produce a different result compared to the default production build.

### Extending the Test Suite

To add new tests, you should find a related playground to the fix or feature (or create a new one). As an example, static assets loading is tested in the [assets playground](https://github.com/vitejs/vite/tree/main/playground/assets). In this Vite app, there is a test for `?raw` imports with [a section defined in the `index.html` for it](https://github.com/vitejs/vite/blob/main/playground/assets/index.html#L121):

```html
<h2>?raw import</h2>
<code class="raw"></code>
```

This will be modified [with the result of a file import](https://github.com/vitejs/vite/blob/main/playground/assets/index.html#L151):

```js
import rawSvg from './nested/fragment.svg?raw'
text('.raw', rawSvg)
```

...where the `text` util is defined as:

```js
function text(el, text) {
  document.querySelector(el).textContent = text
}
```

In the [spec tests](https://github.com/vitejs/vite/blob/main/playground/assets/__tests__/assets.spec.ts#L180), the modifications to the DOM listed above are used to test this feature:

```js
test('?raw import', async () => {
  expect(await page.textContent('.raw')).toMatch('SVG')
})
```

## Note on Test Dependencies

In many test cases, we need to mock dependencies using `link:` and `file:` protocols. `pnpm` treats `link:` as symlinks and `file:` as hardlinks. To test dependencies as if they were copied into `node_modules`, use the `file:` protocol. Otherwise, use the `link:` protocol.

For a mock dependency, make sure you add a `@vitejs/test-` prefix to the package name. This will avoid possible issues like false-positive alerts.

## Debug Logging

You can set the `DEBUG` environment variable to turn on debugging logs (e.g. `DEBUG="vite:resolve"`). To see all debug logs, you can set `DEBUG="vite:*"`, but be warned that it will be quite noisy. You can run `grep -r "createDebugger('vite:" packages/vite/src/` to see a list of available debug scopes.

## Pull Request Guidelines

- Checkout a topic branch from a base branch (e.g. `main`), and merge back against that branch.

- If adding a new feature:

  - Add accompanying test case.
  - Provide a convincing reason to add this feature. Ideally, you should open a suggestion issue first, and have it approved before working on it.

- If fixing a bug:

  - If you are resolving a special issue, add `(fix #xxxx[,#xxxx])` (#xxxx is the issue id) in your PR title for a better release log (e.g. `fix: update entities encoding/decoding (fix #3899)`).
  - Provide a detailed description of the bug in the PR. Live demo preferred.
  - Add appropriate test coverage if applicable.

- It's OK to have multiple small commits as you work on the PR. GitHub can automatically squash them before merging.

- Make sure tests pass!

- No need to worry about code style as long as you have installed the dev dependencies. Modified files are automatically formatted with Prettier on commit (by invoking [Git Hooks](https://git-scm.com/docs/githooks) via [simple-git-hooks](https://github.com/toplenboren/simple-git-hooks)).

- PR title must follow the [commit message convention](./.github/commit-convention.md) so that changelogs can be automatically generated.

## Maintenance Guidelines

> The following section is mostly for maintainers who have commit access, but it's helpful to go through if you intend to make non-trivial contributions to the codebase.

### Issue Triaging Workflow

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="./.github/issue-workflow-dark.png">
  <img src="./.github/issue-workflow.png">
</picture>

### Pull Request Review Workflow

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="./.github/pr-workflow-dark.png">
  <img src="./.github/pr-workflow.png">
</picture>

## Notes on Dependencies

Vite aims to be lightweight, and this includes being aware of the number of npm dependencies and their size.

We use Rollup to pre-bundle most dependencies before publishing! Therefore, most dependencies, even those used in runtime source code, should be added under `devDependencies` by default. This also creates the following constraints that we need to be aware of in the codebase.

### Usage of `require()`

In some cases, we intentionally lazy-require some dependencies to improve start-up performance. However, note that we cannot use simple `require('somedep')` calls since these are ignored in ESM files, so the dependency won't be included in the bundle, and the actual dependency won't even be there when published since they are in `devDependencies`.

Instead, use `(await import('somedep')).default`.

### Think Before Adding a Dependency

Most deps should be added to `devDependencies` even if they are needed at runtime. Some exceptions are:

- Type packages. Example: `@types/*`.
- Deps that cannot be properly bundled due to binary files. Example: `esbuild`.
- Deps that ship their own types that are used in Vite's own public types. Example: `rollup`.

Avoid deps with large transitive dependencies that result in bloated size compared to the functionality it provides. For example, `http-proxy` itself plus `@types/http-proxy` is a little over 1MB in size, but `http-proxy-middleware` pulls in a ton of dependencies that make it 7MB(!) when a minimal custom middleware on top of `http-proxy` only requires a couple of lines of code.

### Ensure Type Support

Vite aims to be fully usable as a dependency in a TypeScript project (e.g. it should provide proper typings for VitePress), and also in `vite.config.ts`. This means technically a dependency whose types are exposed needs to be part of `dependencies` instead of `devDependencies`. However, this also means we won't be able to bundle it.

To get around this, we inline some of these dependencies' types in `packages/vite/src/types`. This way, we can still expose the typing but bundle the dependency's source code.

Use `pnpm run build-types-check` to check that the bundled types do not rely on types in `devDependencies`.

For types shared between client and node, they should be added into `packages/vite/types`. These types are not bundled and are published as is (though they are still considered internal). Dependency types within this directory (e.g. `packages/vite/types/chokidar.d.ts`) are deprecated and should be added to `packages/vite/src/types` instead.

### Think Before Adding Yet Another Option

We already have many config options, and we should avoid fixing an issue by adding yet another one. Before adding an option, consider whether the problem:

- is really worth addressing
- can be fixed with a smarter default
- has workaround using existing options
- can be addressed with a plugin instead

## Release

If you have publish access, the steps below explain how to cut a release for a package. There are two phases for the release step: "Release" and "Publish".

"Release" is done locally to generate the changelogs and git tags:

1. Make sure the git remote for https://github.com/vitejs/vite is set as `origin`.
2. In the `vite` project root `main` branch, run `git pull` and `pnpm i` to get it up-to-date.
3. Run `pnpm release` and follow the prompts to cut a release for a package. It will generate the changelog, a git release tag, and push them to `origin`. You can run with the `--dry` flag to test it out.
4. When the command finishes, it will provide a link to https://github.com/vitejs/vite/actions/workflows/publish.yml.
5. Click the link to visit the page, and follow the next steps below.

"Publish" is done on GitHub Actions to publish the package to npm:

1. Shortly in the workflows page, a new workflow will appear for the released package and is waiting for approval to publish to npm.
2. Click on the workflow to open its page.
3. Click on the "Review deployments" button in the yellow box, a popup will appear.
4. Check "Release" and click "Approve and deploy".
5. The package will start publishing to npm.

## Docs Translation Contribution

To add a new language to the Vite docs, see [`vite-docs-template`](https://github.com/tony19/vite-docs-template/blob/main/.github/CONTRIBUTING.md).
