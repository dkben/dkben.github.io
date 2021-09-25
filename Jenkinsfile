pipeline {
  agent any

  options {
    timeout(time: 10, unit: 'MINUTES')
  }

  parameters {
    choice(name: 'APP_VERSION', choices: ['TEST', 'PROD'], description: '要佈署到那裡？(w3 / digital-ocean)')
    string(name: 'VM_NAME', defaultValue: 'portfolio-hsuweni-info', description: '專案容器名稱')
    string(name: 'GIT_REPO', defaultValue: 'git@github.com:dkben/dkben.github.io.git', description: '專案來源')
    string(name: 'TEST_BRANCH', defaultValue: 'main', description: '測試機使用分支')
    string(name: 'TEST_VM_PORT', defaultValue: '40006', description: '測試機容器埠號')
    string(name: 'TEST_VM_DIR', defaultValue: '/home/ben/projects/portfolio-hsuweni-info', description: '測試機容器資料夾')
  }

  stages {
    stage('Checkout') {
      steps {
        git(url: GIT_REPO, branch: TEST_BRANCH)
      }
    }

    stage('UnitTest') {
      parallel {
        stage('PHP 7.3') {
          agent {
            docker {
              label 'master'
              image 'allebb/phptestrunner-73:latest'
              args '-u root:sudo'
              // Run the container on the node specified at the top-level of the Pipeline, in the same workspace, rather than on a new node entirely:
              reuseNode true
            }
          }
          steps {
            echo 'Running PHP 7.3 tests...'
            println env.WORKSPACE
            //             sh 'php -v'
            //             echo 'Installing Composer'
            //             sh 'curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer'
            //             echo 'Installing project composer dependencies...'
            //             sh 'cd $WORKSPACE && composer install --no-progress'
            //             echo 'Running PHPUnit tests...'
            //             sh 'php $WORKSPACE/vendor/bin/phpunit --coverage-html $WORKSPACE/report/clover --coverage-clover $WORKSPACE/report/clover.xml --log-junit $WORKSPACE/report/junit.xml'
            //             sh 'chmod -R a+w $PWD && chmod -R a+w $WORKSPACE'
            //             junit 'report/*.xml'
          }
        }

        stage('PHP 7.4') {
          agent {
            docker {
              label 'master'
              image 'allebb/phptestrunner-74:latest'
              args '-u root:sudo'
              // Run the container on the node specified at the top-level of the Pipeline, in the same workspace, rather than on a new node entirely:
              reuseNode true
            }
          }
          steps {
            echo 'Running PHP 7.4 tests...'
            println env.WORKSPACE
            //             sh 'php -v'
            //             echo 'Installing Composer'
            //             sh 'curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer'
            //             echo 'Installing project composer dependencies...'
            //             sh 'cd $WORKSPACE && composer install --no-progress'
            //             echo 'Running PHPUnit tests...'
            //             sh 'php $WORKSPACE/vendor/bin/phpunit --coverage-html $WORKSPACE/report/clover --coverage-clover $WORKSPACE/report/clover.xml --log-junit $WORKSPACE/report/junit.xml'
            //             sh 'chmod -R a+w $PWD && chmod -R a+w $WORKSPACE'
            //             junit 'report/*.xml'
          }
        }

      }
    }

    stage('Deployment') {
      parallel {

        stage('Test') {
          stages {
            stage('Deploy') {
              when {
                beforeAgent true
                environment name: 'APP_VERSION', value: 'TEST'
              }            
              agent {
                label 'w3'

              }
              steps {
                println env.WORKSPACE
                dir("${params.TEST_VM_DIR}") {
                  echo 'To Test..'
                }
              }
            }

          }
        }

        stage('Prod') {
          when {
            beforeAgent true
            environment name: 'APP_VERSION', value: 'PROD'
          }
          agent {
            label 'do1'
          }
          steps {
            input "確定要佈署正式機嗎?"
            println env.WORKSPACE
            echo 'To Prod..'
            sh 'touch prod.txt'
          }
        }        

      }
    }

    stage('DeployTest') {
      parallel {
        stage('Test') {
          when {
            beforeAgent true
            environment name: 'APP_VERSION', value: 'TEST'
          }
          agent {
            label 'w3'
          }
          steps {
            echo 'Get Test Response Code..'
            slackSend channel: '#jenkins',
              color: 'good',
              message: "專案：${currentBuild.fullDisplayName}，測試機佈署成功"
          }
        }

        stage('Prod') {
          when {
            beforeAgent true
            environment name: 'APP_VERSION', value: 'PROD'
          }
          agent {
            label 'do1'
          }          
          steps {
            echo 'Get Prod Response Code..'
            slackSend channel: '#jenkins',
              color: 'good',
              message: "專案：${currentBuild.fullDisplayName}，正式機佈署成功"
          }
        }

      }
    }

  }
}