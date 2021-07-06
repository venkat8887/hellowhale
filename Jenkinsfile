pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/srilekhasoma/hellowhale.git', branch:'master'
      }
    }
    
      stage("Build image") {
            steps {
                script {
                    myapp = docker.build("9492261286/hellowhale:${env.BUILD_ID}")
                }
            }
        }
    
      stage("Push image") {
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'dockerHub') {
                            myapp.push("latest")
                            myapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }

    
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy configs: '', kubeConfig: [path: '~/.kube/config'], kubeconfigId: '', secretName: '', ssh: [sshCredentialsId: 'sonar', sshServer: '172.31.46.216'], textCredentials: [certificateAuthorityData: '-----BEGIN CERTIFICATE----- MIIDBjCCAe6gAwIBAgIBATANBgkqhkiG9w0BAQsFADAVMRMwEQYDVQQDEwptaW5p a3ViZUNBMB4XDTIxMDcwNDA3MDkyMVoXDTMxMDcwMzA3MDkyMVowFTETMBEGA1UE AxMKbWluaWt1YmVDQTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBALXL UZSUPM1X/lgFmEOifokzZV2ZWUT4CctJ2KnZMt7LXlS9mtP4hPOCnMsIMxxIiGNo pst3Zy0DMxNI3oIPWu/xEcS1foPl43IPse0QnJ845L5Ax7KUbgf/Uvix1QKCErqf CAa6T4Pyj9yV5TGge/00Mm1BsMa/gDLz/MaRrOpql/dqFmo39v8nMR/hqSl5bmQD bE6VnTs+MwQ4xwpYR/GPfHijFn6dVD3/rjTI6ZOjMmYF7eqde6hsS7+/l9ToBHcB OY2EIllkPz377wCT3MGm8pkcPhXwJS6l/vBIjlU9SmjgURB7+IlIeO0szIOZKf4+ QbGbxSc2b5QkvD9nqrUCAwEAAaNhMF8wDgYDVR0PAQH/BAQDAgKkMB0GA1UdJQQW MBQGCCsGAQUFBwMCBggrBgEFBQcDATAPBgNVHRMBAf8EBTADAQH/MB0GA1UdDgQW BBRexVZx69Ci48XucFyYWlSzRrTerzANBgkqhkiG9w0BAQsFAAOCAQEAg80OeYAy IzTqxKOwufZRzDX4kN7PD3QKma15U2inYXXFaVb+8F3tj5TzOkKoS/n1DPCs7Ehe aXAJ5urHh7JIVVwW8R9hwZ87K1L9CGnNwxsprrtHGNYK8KZf4F9Id40AlQvRPLry 6w5fnQu2coW1usGgpWgcp5vSPL+XYlR4U1DPOY8XkqGaqLsR1mjwZecfNLhFjF6I 29Oagrg/6KxJ/vz5YJEtzXDRf6t6aBfiCwn8//u/khHV4Ksrjl1q65IZ69dt1jG7 Y822am1uoqcKoRZnnBk8a0kOfIUtJj3StAvPZp2DGvd2z5AbGfhiG8D5kZRpXv8F 3b42eHPbnTWyyQ== -----END CERTIFICATE-----', clientCertificateData: '-----BEGIN CERTIFICATE----- MIIDITCCAgmgAwIBAgIBAjANBgkqhkiG9w0BAQsFADAVMRMwEQYDVQQDEwptaW5p a3ViZUNBMB4XDTIxMDcwNDA3MDkyM1oXDTIyMDcwNTA3MDkyM1owMTEXMBUGA1UE ChMOc3lzdGVtOm1hc3RlcnMxFjAUBgNVBAMTDW1pbmlrdWJlLXVzZXIwggEiMA0G CSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCcIpkevgc0rBphdovcgtSPLuYS0W89 jJ8NF9wmTYa4WFtTOiphQiJL9OhpPK0F0wiurOwZhHgxtACtYKap815AWUQiOKjE sbPzNrjiYXatWlxREoQ+n/jOsbtsdthXIwQIh1vtQDXtCC3ZUageVt2Ezo/ncCoR y1XHIIOK0eeV7MntKgPKIAFmvuDH/ihInGZwy9Hn3fJ/w0cbdtU8Ohs7A/SvhJbp MxaK5HLSRN4h9siEiWaGleU99UFtOtsGujqN/LsH5ycGzUlhKbN0/bPoYGMtXtvZ Hk5tzdymjnqr8IprGiRn43ROsV0DRpZOmyAOCheanmoUt1GI/iKpkryRAgMBAAGj YDBeMA4GA1UdDwEB/wQEAwIFoDAdBgNVHSUEFjAUBggrBgEFBQcDAQYIKwYBBQUH AwIwDAYDVR0TAQH/BAIwADAfBgNVHSMEGDAWgBRexVZx69Ci48XucFyYWlSzRrTe rzANBgkqhkiG9w0BAQsFAAOCAQEALL5h3eKEgg3+pmh0tYEH65yF0DDa08x9HV4h QW27JWO88SkcCMRPJXceJu84RNqjt3mw7qOKxPVYPKDaYffeZgwJL3nAbw9uynzD sdVc3n06OakRmt370Faija1wVTDKu3NrHYXYg2WaLJzmrtSxj6Nfs7t6QA9I9ecG lR60umnBmteMJnzaOuWPUdIObrPnE2BF6+U/QTwj/MnPko5RDJVGzsNSqWbXxENY JMSgeDhhNSKpQMhsXvw+/znakE7YUnEFLl93OEWsxjtdiaa+YGtuuM5R76eyblRn 5KwHBrHKlHzMpozYYGLF+ttD18sZM60M3mXwXZ7u69NQ7gpNdg== -----END CERTIFICATE-----', clientKeyData: '-----BEGIN RSA PRIVATE KEY----- MIIEowIBAAKCAQEAnCKZHr4HNKwaYXaL3ILUjy7mEtFvPYyfDRfcJk2GuFhbUzoq YUIiS/ToaTytBdMIrqzsGYR4MbQArWCmqfNeQFlEIjioxLGz8za44mF2rVpcURKE Pp/4zrG7bHbYVyMECIdb7UA17Qgt2VGoHlbdhM6P53AqEctVxyCDitHnlezJ7SoD yiABZr7gx/4oSJxmcMvR593yf8NHG3bVPDobOwP0r4SW6TMWiuRy0kTeIfbIhIlm hpXlPfVBbTrbBro6jfy7B+cnBs1JYSmzdP2z6GBjLV7b2R5Obc3cpo56q/CKaxok Z+N0TrFdA0aWTpsgDgoXmp5qFLdRiP4iqZK8kQIDAQABAoIBAQCZrmhe3RaEnt38 js3Nh60nHjdx0FmZEJ/BKHoV7XssWjPR8M+kGY9eijp00zdPI1BJdoWR/FS+P3nn Ldn+MEDWP8cTlAdyS6NfQr6qfNpueSGi3wHyDk29TS247iW1Zw7iQjGWjfxGSiWu 4XQEIOY7gYIdgMa36xeMP5Gag09avkTUEKulvQsOt2aqhRWEBLLbC/mzNBUWmnfX EctCbuJj/p3jmQgstBmKq9AmXdibjBc8nZIRWyjv3XOhuW92M6B295cop+hGveD6 mKuW9deX6+MwyNis/S+LdGcDUZ3LLSDqNATNo3z77WDcMe9+/fcN+Vi5PcXD9s45 tL9zJwx9AoGBAMgcsWQ98mrbn9E4Vk/1EO6vTj2grvGxbBLrdewluenwvC9NiROv Uf1awmGJJmweS3CgeO2qIu03M1ky7d/8PmdIHLf1g3Qc0H6T8QzI0Rbz/K+FBFIW cI7xVbd9O0NSrc3PA12i9RRES+QTDy19tqSOGOShhDmVJWdL7SaaiB7jAoGBAMe9 tbZikKHb2BHT2C6VJvbn09hHVdSU5Lyj8Uv/2gJ9SMaJaA7A+t2Df84/erPe/TUp xNgws2SwgdUukcJP0nHGYoF4M9+/jQ/6zz7OC1KWtawHAvtdex5yhz1d5efb9Set HUbkRAi6BQiCvI5PraQD2wcoZcxngJp25noKh/z7AoGAWRJEd10HcT7uxR6xdIed kNBhIBdMp3IUq9s4svMb7KBl8xws/qET+pSSXv3AJ3HYnHohOZB4WWQvq+16ai+J y0kS12MlruJAf4b0TX95aiESAUJQ6QTp9wY+5ByO62l6yVfypJQrSGkZ6pv9Ln99 c4N4WxP1mffkHTVzirJQEocCgYA3PmMZtJ0oBzP1ilAKYjpKo8fV07ULfLre6cD9 MiBL+/a64pojKoC373zTwH7hbNU/dPP6j02ulZrzKVQrGASubx4jjOlcAxCy0L2t MzOyffh1QeMzPqGkxCxfbq79t7pQZPLp/oxKlZh6yB36hlMSP/a+PhAZvq61Chmo u2ztLQKBgGrTB6fPtdNCLEQCOAsE9GZKek2VvnuEhH/4P1OpdJ4+2ERiSvKw3fdv vFEulm/P4nK12GUTQSFmYqpRLmGqlC9/Bmgq/TFkX5Uerwe2H1XFNY2wy49Vpci4 9fuzpPTGj5GExWEwzeYB9w0ceZtUCdVsdJ1hJMnhrVp0+JLA5Lu8 -----END RSA PRIVATE KEY-----', serverUrl: 'https://192.168.49.2:8443']
        }
      }
    }

  }
}   
