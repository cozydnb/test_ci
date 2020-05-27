pipeline {
   agent any

   tools {
      // Install the gradle version configured as "G6" and add it to the path.
      gradle "G6"
   }

   stages {
      stage('Preparations') {
         steps {
            // Get some code from a GitHub repository
            git 'https://github.com/cozydnb/test_ci.git'
         }
      }

      stage('build') {
            steps {
                 bat(/gradle build/)
            }
      }

      stage('tests') {
          steps {
              bat(/gradle test/)
          }
      }
   }
}

/*timestamps {
    node('master') {
    def main_branch = ""
        stage('Preparation') {
            cleanWs()
            main_branch = "origin/master"
            target_branch = "master"

            checkout(
                    [$class                           : 'GitSCM',
                     branches                         : [[name: main_branch]],
                     doGenerateSubmoduleConfigurations: false,
                     extensions                       : [[$class             : 'SubmoduleOption',
                                                          disableSubmodules  : false,
                                                          recursiveSubmodules: true, reference: '',
                                                          trackingSubmodules : false],
                                                         [$class : 'PreBuildMerge',
                                                          options:
                                                                  [fastForwardMode: 'FF',
                                                                   mergeRemote    : 'origin',
                                                                   mergeStrategy  : 'DEFAULT',
                                                                   mergeTarget    : target_branch]
                                                         ]],
                     submoduleCfg                     : [],
                     userRemoteConfigs                : [[credentialsId: 'name_pass_id',
                                                          url          : 'https://github.com/cozydnb/test_ci.git']]]

            )

            // main_branch = main_branch.replace("origin/", "")

        }
    }
    stage('Compile') {
        withGradle {
            sh './gradlew jar'
        }
    }

    stage('Unit-tests') {
        withGradle {
            try {
                sh './gradlew build'
            } finally {
                sh './gradlew test'
            }
        }
    }

    //-------------------------DEPLOY PART--------------------------------------------------

    def deploy_required = false
    def html_path = "/var/www/html"
    String repo_path = "$html_path/repository"
    node('arm_30') {
        gitlabCommitStatus("Check for deploy") {
            stage('Check for deploy') {
                cleanWs()
                checkout(
                        [$class                           : 'GitSCM',
                         branches                         : [[name: main_branch]],
                         doGenerateSubmoduleConfigurations: false,
                         extensions                       : [[$class             : 'SubmoduleOption',
                                                              disableSubmodules  : true,
                                                              recursiveSubmodules: true, reference: '',
                                                              trackingSubmodules : false]],
                         submoduleCfg                     : [],
                         userRemoteConfigs                : [[credentialsId: 'pHAPTCI_gitlab',
                                                              url          : 'ssh://git@git.huawei.com:2222/' +
                                                                      'program-analysis-group-russia-rd/HPAT.git']]]
                )
                def new_version = false
                withGradle {
                    new_version = sh(script: "gradle -q checkPluginVersion -PupdxmlPath=$repo_path/updatePlugins.xml",
                            returnStdout: true).toBoolean()
                }
                if (new_version) {
                    def exist_changelog_descr = false
                    withGradle {
                        exist_changelog_descr = sh(script: "gradle -q checkChangeLogVersion -PchangelogPath=$WORKSPACE/docs/html/test/changelog.html",
                                returnStdout: true).toBoolean()
                    }
                    if (!exist_changelog_descr)
                        error("Wasn't provided changelog for new version")
                    if (main_branch == "master")
                        deploy_required = true
                }
            }
        }
    }
    if (deploy_required) {
        node('master') {
            gitlabCommitStatus("Prepare jar for deploy") {
                stage('Prepare jar for deploy') {
                    stash includes: 'build/libs/HPAT*.jar', name: 'plugin_jar'
                }
            }
        }
        node('arm_30') {
            gitlabCommitStatus("Deploy") {
                stage('Deploy') {
                    def version = ""
                    withGradle {
                        sh "gradle editUpdatePlugin -PupdxmlPath=$repo_path/updatePlugins.xml -PchangelogPath=$WORKSPACE/docs/html/test/changelog.html"
                        version = sh(script: 'gradle properties --no-daemon --console=plain -q | grep "^version:" | awk \'{printf $2}\'',
                                returnStdout: true)
                    }
                    unstash 'plugin_jar'
                    dir("$repo_path/HPAT/$version") {
                        sh 'cp $WORKSPACE/build/libs/*.jar ./'
                    }
                    dir("$html_path") {
                        sh 'cp $WORKSPACE/docs/html/changelog.html ./'
                    }
                    // cleaning
                    sh "rm -rf $repo_path/HPAT/*@tmp"
                    sh "rm -rf $html_path/*@tmp"
                }
            }
        }
    }
}*/
