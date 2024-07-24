# Kubectl
- 쿠버네티스 클러스터를 제어하기 위한 CLI 툴

- 기본 형식
``` sh
kubectl [command] [TYPE] [NAME] [flags]
```
1) command
	\- 하나 이상의 리소스에서 수행하려는 동작을 지정
	\- ex) create, get, describe, delete
2) TYPE
	\- 리소스 타입을 지정
	\- 대소문자를 구분하지 않음
	\- 단수형, 복수형 또는 약어 형식 지정 가능
	
``` ad-info
title: TYPE 종류
collapse: close

|type 명     | 약어     |

| --- | --- |

| bindings  |     |

|componentstatuses| cs|

|configmaps|cm|

|endpoints|ep|

|events|ev|

|limitranges|limits|

|namespaces|ns|

|nodes|no|

|persistentvolumeclaims|pvc|

|persistentvolumes|pv|

|pods|po|

|podtemplates| |

|replicationcontrollers|rc|

|resourcequotas|quota|

|secrets| |

|serviceaccounts|sa|

|services|svc|

|mutatingwebhookconfigurations| |

|validatingwebhookconfigurations| |

|customresourcedefinitions|crd,crds|

|apiservices| |

|controllerrevisions| |

|daemonsets|ds|

|deployments|deploy|

|replicasets|rs|

|statefulsets|sts|

|tokenreviews| |

|localsubjectaccessreviews| |

|selfsubjectaccessreviews| |

|selfsubjectrulesreviews| |

|subjectaccessreviews| |

|horizontalpodautoscalers|hpa|

|cronjobs|cj|

|jobs| |

|certificatesigningrequests|csr|

|leases| |

|endpointslices| |

|events|ev|

|flowschemas| |

|prioritylevelconfigurations| |

|ingressclasses| |

|ingresses|ing|

|networkpolicies|netpol|

|runtimeclasses| |

|poddisruptionbudgets|pdb|

|podsecuritypolicies|psp|

|clusterrolebindings| |

|clusterroles| |

|rolebindings| |

|roles| |

|priorityclasses|pc|

|csidrivers| |

|csinodes| |

|csistoragecapacities| |

|storageclasses|sc|

|volumeattachments||

```
	
### 📌 주요 명령어
---
##### 🏷️ apply
- 리소스 생성
- 리소스가 정의된 YAML 파일을 이용해 리소스 생성 (파일뿐만아니라 URL도 가능)

``` sh
kubectl apply -f [YAML 파일명 of URL]
```

##### 🏷️ 