LOCAL_PATH = os.getenv("LOCAL_PATH", default='.')
NAMESPACE = os.getenv("NAMESPACE", default='dev')
WAIT_TIMEOUT = os.getenv("WAIT_TIMEOUT", default='10m00s')
SOURCE_IMAGE=os.getenv("SOURCE_IMAGE", default='taprepoaz123.azurecr.io/tap/workloads/tanzu-java-web-app-fatih-dev')
TYPE = os.getenv("TYPE", default='web')

k8s_custom_deploy(
    'tanzu-java-web-app-fatih',
    apply_cmd="tanzu apps workload apply -f config/workload.yaml --update-strategy replace --debug --live-update" +
               " --namespace " + NAMESPACE +
    " --wait-timeout " + WAIT_TIMEOUT +
    " --type " + TYPE +
    " --yes --output yaml" + " --source-image " + SOURCE_IMAGE + " --local-path " + LOCAL_PATH ,
    delete_cmd="tanzu apps workload delete -f config/workload.yaml --namespace " + NAMESPACE + " --yes",
    deps=['pom.xml', './target/classes'],
    container_selector='workload',
    live_update=[
      sync('./target/classes', '/workspace/BOOT-INF/classes')
    ]
)

k8s_resource('tanzu-java-web-app-fatih', port_forwards=["8080:8080"],
            extra_pod_selectors=[{'carto.run/workload-name': 'tanzu-java-web-app-fatih', 'app.kubernetes.io/component': 'run'}])
