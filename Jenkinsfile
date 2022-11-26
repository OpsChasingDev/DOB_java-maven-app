pipeline{
    agent any
    environment {
        NEW_VERSION = '1.3.0'
    }
    stages{
        stage("Build Application"){
            steps{
                echo "========executing Build========"
                echo "building version ${NEW_VERSION}"
            }
        }
        stage("Test Application"){
            steps{
                echo "========executing Test========"
            }
        }
        stage("Deploy Application"){
            steps{
                echo "========executing Deploy========"
            }
        }
    }
}