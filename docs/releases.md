# Releases

Vite suis la norme [Semantic Versioning](https://semver.org/). Vous pouvez voir la dernière version stable de Vite sur la [page npm de Vite](https://www.npmjs.com/package/vite).

L'historique des releases passées est [disponible sur GitHub](https://github.com/vitejs/vite/blob/main/packages/vite/CHANGELOG.md).

## Cycle des releases

Vite n’a pas de cycle de release fixe.

- Les releases de **Patch** sont publiés quand cela est nécessaire (généralement toutes les semaines).
- Les releases **Mineures** contiennent de nouvelles fonctionnalités et sont publiées quand cela est nécessaire. Les releases **Mineures** ont toujours une phase de pré-release bêta (généralement tous les deux mois).
- Les releases **Majeures** sont généralement alignées avec le [calendrier de fin de vie de Node.js](https://endoflife.date/nodejs) et seront annoncées à l’avance. Ces releases passeront par des discussions longues avec l’écosystème et auront une phase de pré-release alpha et bêta (généralement tous les ans).

La portée des versions de Vite supportée par l’équipe Vite est déterminée automatiquement par :

- **La version Mineur actuelle** reçoit des correctifs réguliers.
- **La version Majeur précédente** (seulement pour sa dernière mineure) et **la version Mineur précédente** reçoivent des correctifs importants et des correctifs de sécurité.
- **L'avant dernière version Majeure** (seulement pour sa dernière mineure) et **l'avant dernière version Mineure** reçoivent des correctifs de sécurité.
- Toutes les versions avant celles-ci ne sont plus supportées.

Par exemple, si la dernière version de Vite est la version 5.4.10 :

- Les correctifs réguliers sont publiés pour`vite@5.3`.
- Les correctifs importants et les correctifs de sécurité sont rétroporté à `vite@4`et`vite@5.2`.
- Les correctifs de sécurité sont également rétroporté à`vite@3`, et`vite@5.1`.
- `vite@2`et `vite@5.0`ne sont plus supportés. Les utilisateurs doivent mettre à jour leur version pour recevoir des mises à jour.

Nous recommandons de mettre à jour Vite régulièrement. Consultez les [guides de migration](https://fr.vite.dev/guide/migration.html) lorsque vous mettez à jour vers chaque Majeur. L’équipe Vite travaille en étroite collaboration avec les principaux projets de l’écosystème pour garantir la qualité des nouvelles versions. Nous testons les nouvelles versions de Vite avant de les publier via le projet [vite-ecosystem-ci](https://github.com/vitejs/vite-ecosystem-ci). La plupart des projets utilisant Vite devraient être en mesure de proposer un support ou de migrer vers de nouvelles versions dès qu’elles sont publiées.

## Cas à la marge du Semantic Versioning

### Définitions TypeScript

Il est possible que nous incorporions des changements incompatibles avec les définitions TypeScript, entre les versions mineur. Cela est dû au fait que :

- Parfois, TypeScript lui-même publie des changements incompatibles entre les versions mineures, et nous devons ajuster les types pour supporter de nouvelles versions de TypeScript.
- Occasionnellement nous devons adopter des fonctionnalités qui ne sont disponibles que dans une nouvelle version de TypeScript, ce qui augmente la version minimale requise de TypeScript.
- Si vous utilisez TypeScript, vous pouvez utiliser une version semver qui verrouille la dernière version mineure et mettre à jour manuellement lorsqu’une nouvelle version mineure de Vite est publiée.

### esbuild

[esbuild](https://esbuild.github.io/) est pre-1.0.0 et parfois il a un changement majeur que nous devons inclure pour avoir accès aux nouvelles fonctionnalités et améliorations de performance. Nous pourrions augmenter la version d’esbuild dans une version mineure de Vite.

### Node.js non-LTS versions

Les versions de Node.js non LTS (les numéros impairs) ne sont pas testées comme faisant partie de Vite's CI, mais elles devraient encore fonctionner avant leur [fin de vie](https://endoflife.date/nodejs).

## Pré-releases

Les releases mineures passent généralement par un nombre non fixe de bêta. Les releases majeures passeront par une phase alpha et une phase bêta.

Les pré-releases permettent aux utilisateurs et aux contributeurs de l’écosystème de faire des tests d’intégration et de stabilité, et de fournir des retours. N'utilisez pas les pré-releases en production. Toutes les pré-releases sont considérées comme instables et peuvent contenir des changements incompatibles entre les versions. Veuillez toujours épingler (pin) la version exacte lorsque vous utilisez les pré-releases.

## Dépréciations

Nous déprécions périodiquement des fonctionnalités qui ont été remplacées par de meilleures alternatives dans les releases mineures. Les fonctionnalités dépréciées continueront de fonctionner avec un avertissement de type ou avec un message de journalisation. Elles seront supprimées dans la prochaine version majeure après avoir été dépréciées. Lors de chaque majeur, le [Guide de migration](https://fr.vite.dev/guide/migration.html) listera ces suppression et documentera une méthode de mise à jour, pour chacune d’elles.

## Fonctionnalités expérimentales

Certaines fonctionnalités sont marquées comme expérimentales lorsqu’elles sont publiées dans une version stable de Vite. Les fonctionnalités expérimentales nous permettent de collecter des retours d’utilisation dans le monde réel pour influencer leur conception finale. L'objectif est de permettre aux utilisateurs de fournir des retours en testant ces fonctionnalités en production. Les fonctionnalités expérimentales sont, par définition, considérées comme instables et ne doivent être utilisées que dans un environnement contrôlé. Ces fonctionnalités peuvent changer entre deux versions Mineures, aussi les utilisateurs doivent épingler (pin) la version exacte de Vite lorsqu'ils en dépendent. Nous créerons une [discussion GitHub](https://github.com/vitejs/vite/discussions/categories/feedback?discussions_q=is%3Aopen+label%3Aexperimental+category%3AFeedback) pour chaque fonctionnalité expérimentale.
