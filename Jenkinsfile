pipeline{
	agent any
	tools{
		maven 'maven'
		jdk 'java'	
	}
	stages{
		stage('GetDiff'){
				steps{
					sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''#!/bin/bash

					cd /etc/ansible/181/core_deployment;
					echo "-" > file.yml
					echo " corelib:" >> file.yml
					echo "  - aop.jar" >> file.yml
					find /opt/estel/mcommerce/ -type f -name "*.*" -newermt "$date" -exec ls  {} \\; |grep blu_lib_merge  |grep -vE \'2020|-backup|_backup|_bkp|-bkp|dbmodule.jar|aop.jar\' |awk -F "/" \'{print $6}\' |sed -e \'s/^/  - /g\' >> file.yml


					echo "-" >> file.yml
					echo " coreglocm:" >> file.yml
					echo "  - core.xml" >> file.yml
					find /opt/estel/mcommerce/ -type f -name "*.*" -newermt "$date" -exec ls  {} \\; |grep core_glocm  |grep -vE \'nohup.out|core.lic|2020|-backup|_backup|_bkp|-bkp|logs|core.xml\' |awk -F "/" \'{print $7}\' |sed -e \'s/^/  - /g\' >> file.yml


					echo "-" >> file.yml
					echo " schema:" >> file.yml
					echo "  - agentInfo.xsd" >> file.yml
					find /opt/estel/mcommerce/ -type f -name "*.*" -newermt "$date" -exec ls  {} \\; |grep xsd |grep schema_bluClound |grep -vE \'2020|-backup|_backup|_bkp|-bkp|logs|agentInfo.xsd\' |awk -F "/" \'{print $6}\' |sed -e \'s/^/  - /g\' >> file.yml
					''', execTimeout: 0, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
				}
				
		}
		
		stage('Build'){
				steps{
					sshPublisher(publishers: [sshPublisherDesc(configName: 'ansible server', transfers: [sshTransfer(cleanRemote: false, excludes: '', execCommand: '''
					
					cd /etc/ansible/181/core_deployment;
					ansible-playbook core.yml -i inventory.yml
					
					''', execTimeout: 0, flatten: false, makeEmptyDirs: false, noDefaultExcludes: false, patternSeparator: '[, ]+', remoteDirectory: '', remoteDirectorySDF: false, removePrefix: '', sourceFiles: '')], usePromotionTimestamp: false, useWorkspaceInPromotion: false, verbose: false)])
				}
				
		}

	}

	post{
		always{
			emailext attachLog: true, body: '${DEFAULT_CONTENT}', compressLog: true, subject: '${DEFAULT_SUBJECT}', to: 'omkar.saini@esteltelecom.com'
	   
		}
		success{
			emailext attachLog: true, body: '${DEFAULT_CONTENT}', compressLog: true, subject: '${DEFAULT_SUBJECT}', to: 'omkar.saini@esteltelecom.com'
		}
		failure{
			emailext attachLog: true, body: '${DEFAULT_CONTENT}', compressLog: true, subject: '${DEFAULT_SUBJECT}', to: 'omkar.saini@esteltelecom.com'
		}
		unstable{
			emailext attachLog: true, body: '${DEFAULT_CONTENT}', compressLog: true, subject: '${DEFAULT_SUBJECT}', to: 'omkar.saini@esteltelecom.com'
		}
		changed{
			emailext attachLog: true, body: '${DEFAULT_CONTENT}', compressLog: true, subject: '${DEFAULT_SUBJECT}', to: 'omkar.saini@esteltelecom.com'
		}
		          
	
	}

	
}	
