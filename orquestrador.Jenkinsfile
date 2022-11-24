def PULL_NUMERO
def PULL_URL
def PULL_AUTOR
def CUSTOMWS = "C:/JK/orquestrador"

def EMOJI_OK = ':ok_hand:'
def EMOJI_INDEFINIDO = ':thinking:'
def EMOJI_ERRO = ':sob:'
def ROCKET_USERS = '@bruno.fernandes, @danilo.pereira, @lucas.sigaki'

pipeline {
    agent {
        label {
            label "master"
            customWorkspace "${CUSTOMWS}"
        }
    }
    environment {
		caminho = "${CUSTOMWS}"
		caminhoWindows = "${caminho.replace('/','\\')}\\DLLS";
        
		msbuild = '"C:\\Program Files (x86)\\Microsoft Visual Studio\\2017\\Professional\\MSBuild\\15.0\\Bin\\MSBuild.exe"'

		nuget = '"C:\\Executaveis\\NuGet.exe"'	
		testExecute = "${env.TestExecute}";
		nunit = "${env.NUnit3}"
		parametrosBuild = "/verbosity:m /p:WarningLevel=0 /tv:15.0 /t:Build /p:Configuration=Release /p:BuildInParallel=true"
	}
    stages {
		stage('Restaurar nuget'){
			steps {
			    bat('forfiles /p . /m *.sln /c "cmd /c %nuget% restore @file"')
			}
		}
		
		stage('Build Solutions'){
			steps {
				bat('%msbuild% orquestradorTeamCity.sln %parametrosBuild% /property:OutDir="%caminhoWindows%" ')
			}
		}
		
		stage('Testes Unitarios'){
			steps {
				script {	
					bat('dir "%caminhoWindows%\\*.dll" /s /b | find "Tests.dll" > "%caminhoWindows%\\dlls_teste.txt" ')
					bat('%testExecute% N %nunit% %caminhoWindows%\\dlls_teste.txt "%caminhoWindows%"')					
				}
			}
		}


        stage('Copiar'){
			steps {
				script {
					bat('del %caminhoWindows%\\*.config /q /f ')
					bat('del %caminhoWindows%\\*.xml /q /f ')
					bat('del %caminhoWindows%\\*.txt /q /f ')
					
					try{ bat('copy %caminhoWindows%\\*.* \\\\mga-tc008\\Utilitarios\\Orquestrador /y ')}catch (e) {}
					try{ bat('copy %caminhoWindows%\\*.* \\\\mga-tc002\\c$\\Executaveis\Orquestrador /y ')}catch (e) {}
					try{ bat('copy %caminhoWindows%\\*.* \\\\mga-tc002\\c$\\Blue /y ')}catch (e) {}
					try{ bat('copy %caminhoWindows%\\*.* \\\\mga-tc002\\c$\\Green /y ')}catch (e) {}
					try{ bat('copy %caminhoWindows%\\*.* \\\\mga-tc004\\c$\\Executaveis /y ')}catch (e) {}
					try{ bat('copy %caminhoWindows%\\*.* \\\\mga-tc007\\c$\\Executaveis /y ')}catch (e) {}
					try{ bat('copy %caminhoWindows%\\*.* \\\\mga-tc009\\c$\\Executaveis /y ')}catch (e) {}
					try{ bat('copy %caminhoWindows%\\*.* \\\\mga-tc009\\c$\\Blue /y ')}catch (e) {}
					try{ bat('copy %caminhoWindows%\\*.* \\\\mga-tc009\\c$\\Green /y ')}catch (e) {}
					try{ bat('copy %caminhoWindows%\\*.* \\\\mga-tc010\\c$\\Executaveis /y ')}catch (e) {}
					try{ bat('copy %caminhoWindows%\\*.* \\\\mga-tc101\\c$\\Executaveis /y ')}catch (e) {}
					try{ bat('copy %caminhoWindows%\\*.* \\\\mga-sc001\\c$\\Executaveis /y ')}catch (e) {}
					try{ bat('copy %caminhoWindows%\\*.* \\\\mga-tcdesk002\\c$\\Executaveis /y ')}catch (e) {}
					try{ bat('copy %caminhoWindows%\\*.* \\\\mga-tcdesk001\\c$\\Executaveis /y ')}catch (e) {}
					try{ bat('copy %caminhoWindows%\\*.* \\\\mga-tcdesk03\\c$\\Executaveis /y ')}catch (e) {}
					try{ bat('copy %caminhoWindows%\\*.* \\\\mga-tcdesk03\\c$\\Blue /y ')}catch (e) {}
					try{ bat('copy %caminhoWindows%\\*.* \\\\mga-tcdesk03\\c$\\Green /y ')}catch (e) {}
					try{ bat('copy %caminhoWindows%\\*.* c:\\Executaveis /y ')}catch (e) {}
				}
			}
		}
		
		stage('Limpar diretorio'){
			steps {
				cleanWs notFailBuild: false
			}
		}
    }
    post {
       	unstable {
           	rocketSend channel: "${ROCKET_USERS}", emoji: "${EMOJI_INDEFINIDO}", message: "Não consegui identificar se funcionou ou não - ${env.JOB_NAME}-${env.BUILD_NUMBER} (<${env.BUILD_URL}|Ver log>)", rawMessage: true
       	}
       	failure {
           	rocketSend channel: "${ROCKET_USERS}", emoji: "${EMOJI_ERRO}", message: "Falha ao compilar o Orquestrador! - ${env.JOB_NAME}-${env.BUILD_NUMBER} (<${env.BUILD_URL}|Ver log>)", rawMessage: true
       	}
    }
}
