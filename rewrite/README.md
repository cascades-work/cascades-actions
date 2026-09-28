# Cascades plugin OpenRewrite standard

`io.cascades.rewrite.PluginBuildStandard` is the canonical migration recipe for Cascades JVM plugin repositories.

It performs the following changes:

- enables Gradle build cache and parallel execution through `GradleBestPractices`
- enables the Gradle configuration cache
- migrates explicit dependency versions into `gradle/libs.versions.toml`
- removes redundant versions already managed by the Cascades platform/BOM
- normalizes the Gradle wrapper to 9.6
- generates a local `buildSrc` convention plugin named `io.cascades.plugin-conventions`
- moves common repository, Java 25 toolchain, compiler, JUnit Platform, and JaCoCo setup into that convention plugin
- removes the duplicated root build setup that the convention plugin replaces

The local convention plugin is deliberate. It avoids adding a private plugin-registry bootstrap dependency while the migration is being rolled out. Once all plugin repositories are standardized, this buildSrc plugin can be promoted to a versioned organization convention plugin without changing the plugin id.

## Run

The init script requires Code Genome credentials because current OpenRewrite Gradle artifacts are distributed from the Code Genome repository:

```bash
export CODE_GENOME_USERNAME=...
export CODE_GENOME_TOKEN=...
export CASCADES_REWRITE_CONFIG=/absolute/path/to/cascades-plugin-standard.yml
./gradlew --init-script /absolute/path/to/init.gradle rewriteRun
./gradlew check
```

Always run the recipe on a branch and require the repository's normal CI before merge. Plugin-specific test JVM arguments and other nonstandard test settings are intentionally preserved.
