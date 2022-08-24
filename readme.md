<h1>Errors Faced By Me</h1>
<p>These are all the errors I faced while following the <a href="https://www.youtube.com/watch?v=X48VuDVv0do&t=6s">Kubernetes tutorial by Techworld with Nana</a></p> 
<br/>

<section>
    <h2><strong>1. "context deadline exceeded"</strong></h2>
    <br/>
    <div style="width: 90%; margin: 0 auto;">
        <p>When pulling the image from docker hub, the pod keeps giving content timeout error.</p>
        <br/>
        <p><span style="margin-right: 20px;"><strong>Expected Behavior:</strong></span>Pods Created</p>
        <p><span style="margin-right: 20px;"><strong>Resulting Behavior:</strong></span>Failed to pull image "mongo:4.4-rc-focal": rpc error: code = Unknown desc = context deadline exceeded</p>
        <br/>
        <h2><strong>Solution That Worked</strong></h2>
        <br/>
        <p>Connecting to a faster internet. This way the image gets downloaded within the timeout.</p>
        <h2><strong>Other Solutions</strong></h2>
        <br>
        <p>There are 3 solutions as mentioned in <a href="https://stackoverflow.com/questions/72989275/failed-to-pull-image">this article</a>:</p>
        <ul>
            <li>Keep trying until the image eventually gets pulled</li>
            <li>Manually pull the image so pods can get it locally <strong>(Fastest)</strong><br/>
            <strong>Note: </strong> This can be done as shown in <a href="https://stackoverflow.com/questions/42564058/how-to-use-local-docker-images-with-minikube">this article</a>:<br/>
            <em style="margin-left: 20px">eval $(minikube docker-env)</em><br/>
            <em style="margin-left: 20px">docker pull image:tag</em>
            </li>
            <li>Increase timeout using --runtime-request-timeout flag <strong>(Prefered/Generic)</strong><br/>
            <strong>Note: </strong> for minikube this will be:<br/> 
            <em style="margin-left: 20px">minikube start --driver=docker --extra-config=kubelet.runtime-request-timeout=10m</em><br/>
            <strong>Note: </strong> This did not work for me
            </li>
        </ul>
        <br/>
        <p><strong>Note: </strong> I did find <a href="https://groups.google.com/g/kubernetes-sig-storage-bugs/c/3KHBY7zF6z0#:~:text=you%20set%20the-,%2D%2Dimage%2Dpull%2Dprogress%2Ddeadline,-on%20the%20kubelet">another article</a> which tells us to set the --image-pull-progress-deadline flag on kubelet in the Daemon Arguments, <a href="https://support.huaweicloud.com/intl/en-us/cce_faq/cce_faq_00015.html#cce_faq_00015__section1962218412226:~:text=end%20of%20the-,DAEMON_ARGS,-parameter.%2030m">as shown here</a>. However, I could not find this flag</p>
    </div>
</section>
<br/>

<section>
    <h2><strong>2. Pod crash loop after applying secret</strong></h2>
    <br/>
    <div style="width: 90%; margin: 0 auto;">
        <p>When applying the mongo.yaml file, it keeps getting stuck in a crash loop. This seems to only happen when using valueFrom tag to get data from secret file. When I give value directly it does not happen.</p>
        <br/>
        <p><span style="margin-right: 20px;"><strong>Expected Behavior:</strong></span>Pods Created</p>
        <p><span style="margin-right: 20px;"><strong>Resulting Behavior:</strong></span>Status switching between Error and CrashLoop</p>
        <br/>
        <h2><strong>Solution</strong></h2>
        <br>
        <p>The values put in secret file need to be base 64 encoded. I was creating them using<br/>
        <em style="margin-left: 20px">echo "username" | base64</em><br/>
        But there is an issue with this, which is pointed out in the tutorial as well, '-n' flag needs to be added as well. This would be:<br/>
        <em style="margin-left: 20px">echo -n "username" | base64</em><br/>
        <p>
    </div>
</section>
<br/>

<section>
    <h2><strong>3. Making certificates for https</strong></h2>
    <br/>
    <div style="width: 90%; margin: 0 auto;">
        <p>Getting actual certificates via kubernetes certificates API is a tedious and long process. So I need a shortcut</p>
        <h2><strong>Solution</strong></h2>
        <br>
        <p>Use this command(<a href="https://kubernetes.github.io/ingress-nginx/user-guide/tls/">source</a>) to generate private key and self-signed certificate:<br/>
        <em style="margin-left: 20px">openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout ${KEY_FILE} -out ${CERT_FILE} -subj "/CN=${HOST}/O=${HOST}"</em><br/>
        Now you can add them to your tls secrets file<br/>
        <p>
        <p><strong>Note: </strong> Since we are using slef-signed certs, it will showup as Nott Secure on browsers. </p>
    </div>
</section>
<br/>

<section>
    <h2><strong>4. Attaching PV to Mongo DB</strong></h2>
    <br/>
    <div style="width: 90%; margin: 0 auto;">
        <p>I have made the pv.yaml and pvc.yaml files and also made relevent changes to my deployment.yaml file, but data does no persist.</p>
        <p><span style="margin-right: 20px;"><strong>Expected Behavior:</strong></span>Data should persist even after minikube restarts</p>
        <p><span style="margin-right: 20px;"><strong>Resulting Behavior:</strong></span>DB changes are lost</p>
        <br/>
        <h2><strong>Solution</strong></h2>
        <br>
        <p></p>
    </div>
</section>
<br/>
