pipeline {
  agent {
    label 'arma-reforger'
  }

  stages {
    stage('Setup') {
      steps {
        bat 'mkdir pack'
        bat 'mkdir profiles'
      }
    }

    stage('Checkout') {
      steps {
        git url: 'https://github.com/acemod/ACE-Anvil.git', branch: 'master', changelog: true, poll: true
      }
    }

    stage("Build") {
      steps {
        script {
          def addon = "ACE_AllInOne"
          bat "\"%ARMA_REFORGER_TOOLS%\\Workbench\\ArmaReforgerWorkbenchSteamDiag.exe\" -enableWARP -noThrow -wbModule=ResourceManager -addonsDir \"%ARMA_REFORGER%/addons/,%WORKSPACE%/addons/\" -addons \"${addon}\" -profile \"%WORKSPACE%/profiles/${addon}-pack\" -packAddon -packAddonDir \"%WORKSPACE%/pack/${addon}\""
        }
      }
    }
  }

  post { 
    always {
      archiveArtifacts artifacts: "profiles/**/*.log", allowEmptyArchive: true
    }
  }
}

def findAddons() {
  dir('addons') {
    findFiles().each { file ->
      if (file.isDirectory()) {
        def addon = file.toString().replace("/", "")
        bat "\"%ARMA_REFORGER_TOOLS%\\Workbench\\ArmaReforgerWorkbenchSteamDiag.exe\" -enableWARP -noThrow -wbModule=ResourceManager -addonsDir \"%ARMA_REFORGER%/addons/,%WORKSPACE%/addons/\" -addons \"${addon}\" -profile \"%WORKSPACE%/profiles/${addon}-pack\" -packAddon -packAddonDir \"%WORKSPACE%/pack/${addon}\""
      }
    }
  }
}
