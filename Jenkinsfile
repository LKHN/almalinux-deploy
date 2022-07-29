def erecipients = "TO_BE_FILLED"
def ebody = """
${currentBuild.fullDisplayName} / ${currentBuild.number}
Check url: ${currentBuild.absoluteUrl}.
"""

def supported_9_machine_names = ["oracle9", "rhel9", "rocky9"]
def supported_8_machine_names = ["oracle8", "rhel8", "rocky8"]
def supported_centos_stream = ["centos8stream", "centos9stream"]
def legacy_8_5_machine_names = ["centos8-5", "oracle8-5", "rhel8-5", "rocky8-5"]
def legacy_8_4_machine_names = ["centos8-4", "oracle8-4", "rhel8-4", "rocky8-4"]

pipeline {
	agent {
		node {
			label 'libvirt'
		}
	}
	stages {
		stage("Migrate supported 9 stable systems to AlmaLinux 9") {
			steps {
				catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {
					script {
						parallel supported_9_machine_names.collectEntries {
							vagrant_machine -> ["${vagrant_machine}": {
								stage("$vagrant_machine") {
									sleep(5 * Math.random())
									sh("vagrant up $vagrant_machine")
									sh("vagrant ssh $vagrant_machine -c \"sudo /home/vagrant/almalinux-deploy/almalinux-deploy.sh\"")
									sleep(5 * Math.random())
									sh("vagrant destroy $vagrant_machine -f")
								}
							}]
						}
					}
				}
			}
		}
		stage("Migrate supported 8 stable systems to AlmaLinux 8") {
			steps {
				catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {
					script {
						parallel supported_8_machine_names.collectEntries {
							vagrant_machine -> ["${vagrant_machine}": {
								stage("$vagrant_machine") {
									sleep(5 * Math.random())
									sh("vagrant up $vagrant_machine")
									sh("vagrant ssh $vagrant_machine -c \"sudo /home/vagrant/almalinux-deploy/almalinux-deploy.sh\"")
									sleep(5 * Math.random())
									sh("vagrant destroy $vagrant_machine -f")
								}
							}]
						}
					}
				}
			}
		}
		stage("Migrate supported CentOS Stream to equivalent AlmaLinux") {
			steps {
				catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {
					script {
						parallel supported_centos_stream.collectEntries {
							vagrant_machine -> ["${vagrant_machine}": {
								stage("$vagrant_machine") {
									sleep(5 * Math.random())
									sh("vagrant up $vagrant_machine")
									sh("vagrant ssh $vagrant_machine -c \"sudo /home/vagrant/almalinux-deploy/almalinux-deploy.sh -d\"")
									sleep(5 * Math.random())
									sh("vagrant destroy $vagrant_machine -f")
								}
							}]
						}
					}
				}
			}
		}
		stage("Migrate legacy 8.5 stable systems to AlmaLinux 8") {
			steps {
				catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {
					script {
						parallel legacy_8_5_machine_names.collectEntries {
							vagrant_machine -> ["${vagrant_machine}": {
								stage("$vagrant_machine") {
									sleep(5 * Math.random())
									sh("vagrant up $vagrant_machine")
									sh("vagrant ssh $vagrant_machine -c \"sudo /home/vagrant/almalinux-deploy/almalinux-deploy.sh\"")
									sleep(5 * Math.random())
									sh("vagrant destroy $vagrant_machine -f")
								}
							}]
						}
					}
				}
			}
		}
		stage("Migrate legacy 8.4 stable systems to AlmaLinux 8") {
			steps {
				catchError(buildResult: 'FAILURE', stageResult: 'FAILURE') {
					script {
						parallel legacy_8_4_machine_names.collectEntries {
							vagrant_machine -> ["${vagrant_machine}": {
								stage("$vagrant_machine") {
									sleep(5 * Math.random())
									sh("vagrant up $vagrant_machine")
									sh("vagrant ssh $vagrant_machine -c \"sudo /home/vagrant/almalinux-deploy/almalinux-deploy.sh\"")
									sleep(5 * Math.random())
									sh("vagrant destroy $vagrant_machine -f")
								}
							}]
						}
					}
				}
			}
		}
	}
	post {
		success {
			echo 'Pipeline finished'
		}
		failure {
			echo 'Pipeline failed'
			mail to: erecipients,
				subject: "Pipeline failed: ${currentBuild.fullDisplayName}",
				body: ebody
		}
		always {
			echo 'Running "vagrant destroy -f"'
			sh("vagrant destroy -f")
			cleanWs()
		}
	}
}
