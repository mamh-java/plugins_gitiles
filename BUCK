include_defs('//bucklets/gerrit_plugin.bucklet')
include_defs('//bucklets/maven_jar.bucklet')

define_license('commonmark')
define_license('autolink')

genrule(
  name = 'gitiles',
  cmd = ' && '.join([
    'cp $(location :gitiles__base) $OUT',
    'unzip -qd $TMP $(location :gitiles-servlet) "com/google/gitiles/static/*"',
    'cd $TMP/com/google/gitiles',
    'mv static +static',
    'zip -Drq $OUT -g . -i "+static/*"',
  ]),
  visibility = ['PUBLIC'],
  out = 'gitiles.jar',
)

gerrit_plugin(
  name = 'gitiles',
  srcs = glob(['src/main/java/**/*.java']),
  deps = [
    ':gitiles-servlet',

    # Deps only needed by Gitiles.
    ':commons-lang3',
    ':commonmark',
    ':cm-autolink',
    ':gfm-tables',
    ':gfm-strikethrough',

  ],
  resources = glob(['src/main/resources/**/*']),
  manifest_entries = [
    'Gerrit-PluginName: gitiles',
    'Gerrit-Module: com.googlesource.gerrit.plugins.gitiles.Module',
    'Gerrit-HttpModule: com.googlesource.gerrit.plugins.gitiles.HttpModule',

    # Gitiles uses /repo to access a repo, so the default plugin layout would
    # disallow repos named "static" or "Documentation". Paths starting with +
    # are reserved by Gitiles and can't match repos.
    'Gerrit-HttpStaticPrefix: +static',
    'Gerrit-HttpDocumentationPrefix: +Documentation',
  ],
  visibility = [],
  target_suffix = '__base'
)

java_library(
  name = 'classpath',
  deps = [':gitiles__plugin'],
)

maven_jar(
  name = 'gitiles-servlet',
  id = 'com.google.gitiles:gitiles-servlet:0.1-11',
  sha1 = 'c8fe6a48472a51607103a4b10214867bcc1c63f5',
  deps = [':prettify'],
  license = 'Apache2.0',
  repository = GERRIT,
  visibility = [],
)

# prettify must match the version used in Gitiles
maven_jar(
  name = 'prettify',
  id = 'prettify:java-prettify:1.2.1',
  sha1 = '29ad8d072f9d0b83d1a2e9aa6ccb0905e6d543c6',
  license = 'Apache2.0',
  repository = GERRIT,
  visibility = [],
)

maven_jar(
  name = 'commons-lang3',
  id = 'org.apache.commons:commons-lang3:3.1',
  sha1 = '905075e6c80f206bbe6cf1e809d2caa69f420c76',
  license = 'Apache2.0',
  visibility = [],
)

COMMONMARK_VERSION = '0.6.0'
maven_jar(
  name = 'commonmark',
  id = 'com.atlassian.commonmark:commonmark:' + COMMONMARK_VERSION,
  sha1 = '5df3f6fa3073966620685924aa22d08ece7213f2',
  license = 'commonmark',
  local_license = True,
)

maven_jar(
  name = 'cm-autolink',
  id = 'com.atlassian.commonmark:commonmark-ext-autolink:' + COMMONMARK_VERSION,
  sha1 = '4d7e828a4651e2f590b4a059925991be58e62da6',
  license = 'commonmark',
  local_license = True,
  deps = [
    ':commonmark',
    ':autolink',
  ],
)

maven_jar(
  name = 'autolink',
  id = 'org.nibor.autolink:autolink:0.4.0',
  sha1 = '764f7b0147a0675d971a34282dce9ec76b8307c9',
  license = 'autolink',
  local_license = True,
)

maven_jar(
  name = 'gfm-strikethrough',
  id = 'com.atlassian.commonmark:commonmark-ext-gfm-strikethrough:' + COMMONMARK_VERSION,
  sha1 = '75a95aaec77810496de41239bcc773adfb13285f',
  license = 'commonmark',
  local_license = True,
  deps = [':commonmark'],
)

maven_jar(
  name = 'gfm-tables',
  id = 'com.atlassian.commonmark:commonmark-ext-gfm-tables:' + COMMONMARK_VERSION,
  sha1 = 'ae1c701517e8116bc205b561b9b215a53df8abc7',
  license = 'commonmark',
  local_license = True,
  deps = [':commonmark'],
)
