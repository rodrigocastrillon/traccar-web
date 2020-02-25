node ('traccar') {
   stage('Preparation') { // for display purposes
      //git 'https://github.com/rodrigocastrillon/traccar-web.git'
      git 'https://github.com/traccar/traccar-web'
   }
   stage('Stop Server') {
       try {
           def traccarPID = sh(script: "ps -ef|grep traccar|grep -v grep|awk '{ print \$2 }'", returnStdout: true)
           sh "kill -9 ${traccarPID}"
       } catch (e) {
           echo "Traccar server is not running"
       }
   }
   stage('Backup') {
       if (fileExists('/opt/traccar/web')) {
           sh 'mkdir -p /opt/traccar-web_backup/`date +"%d-%m-%Y"`'
           sh 'cp -Rn /opt/traccar/web /opt/traccar-web_backup/`date +"%d-%m-%Y"`'
       } else {
           echo "Traccar not found"
       }
   }
   stage('Deploy') {
      sh "mkdir -p /opt/traccar/web"
      sh "rm -Rf /opt/traccar/web/*"
      sh "cp -R web/* /opt/traccar/web"
   }
   stage('Sart Server') {
       dir('/opt/traccar') {
           sh "java -jar tracker-server.jar traccar.xml &"
       }
   }
}
