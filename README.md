<h1 id="red-hat-openshift-origin-with-contrail-sdn">Red Hat OpenShift Origin with Contrail SDN</h1>
<p>This tutorial walks you through the installation of Red Hat OpenShift container orchestration platform with Contrail SDN as the CNI on Amazon Web Services (AWS).</p>
<p>It leverages AWS's CloudFormation to launch the stack &amp; takes approximately 30 min for the total installation to complete. The stack builds</p>
<ul>
<li>Red Hat OpenShift Origin v3.7</li>
<li>Contrail Networking CNI 5.0</li>
</ul>
<h1 id="prerequisites">Prerequisites</h1>
<ul>
<li><p><a href="https://portal.aws.amazon.com/billing/signup#/start">Create</a> an AWS account if you don't have one. Else <a href="https://aws.amazon.com/console/">Login</a></p></li>
<li><p><a href="https://aws.amazon.com/marketplace/pp/B00O7WM7QW">Subscribe</a> to CentOS AMI on AWS marketplace</p></li>
<li><p><a href="https://github.com/join">Create</a> a GitHub account if you don't have one. Else <a href="https://github.com/login">Login</a></p></li>
</ul>
<h1 id="installation">Installation</h1>
<ul>
<li><p>Click on the button below to launch the stack in AWS</p>
<p><a href="https://us-west-1.console.aws.amazon.com/cloudformation/home?region=us-west-1#/stacks/create/review?filter=active&templateURL=https:%2F%2Fs3-us-west-1.amazonaws.com%2Fcontrail-dev-ops%2Fopenshift-contrail-stack-5.yaml&stackName=openshift-stack" target="_blank"><img alt="Launch Stack" src="https://cdn.rawgit.com/buildkite/cloudformation-launch-stack-button-svg/master/launch-stack.svg"></a></p></li>
<li><p>Once you click on the button, you will be navigated to AWS CloudFormation page. Enter the parameters</p></li>
</ul>
<div class="figure">
<img src="https://github.com/savithruml/cloud-ops/blob/master/aws/cloudformation/openshift/images/1-initiate.jpg" alt="launch-stack" /><p class="caption">launch-stack</p>
</div>
<pre><code>**NOTE:** You can leave most of the parameters set to default

   InstanceType:
     Description: EC2 instance type
     Default: t2.xlarge

   VpcCIDR:
     Description: CIDR block for the VPC
     Default: 10.10.0.0/16

   SubnetCIDR:
     Description: CIDR block for the VPC subnet
     Default: 10.10.10.0/24

   MasterIPv4Address:
     Description: Master instance&#39;s IPv4 Address
     Default: 10.10.10.10

   MinionIPv4Address:
     Description: Minion instance&#39;s IPv4 Address
     Default: 10.10.10.11

   SSHLocation:
     Description: Allow access to EC2 instances from
     Default: 0.0.0.0/0

   InstancePassword:
     Description: Password for the instances

   ContrailBuild:
     Description: Contrail build information
     Default: 5.0

   ContrailRegistry:
     Description: Registry to pull Contrail containers
     Default: hub.juniper.net/contrail

   ContrailRegistryUsername:
     Description: Registry username

   ContrailRegistryPassword:
     Description: Registry password</code></pre>
<ul>
<li>Wait for the stack to complete. You can monitor the resource creation by clicking on the <strong>Events</strong> tab</li>
</ul>
<div class="figure">
<img src="https://github.com/savithruml/cloud-ops/blob/master/aws/cloudformation/openshift/images/2-monitor.jpg" alt="monitor-stack" /><p class="caption">monitor-stack</p>
</div>
<ul>
<li>Once complete, navigate to the <strong>Outputs</strong> tab &amp; copy the ShellURL value. Login to the instance using the ShellURL &amp; the password you set</li>
</ul>
<div class="figure">
<img src="https://github.com/savithruml/cloud-ops/blob/master/aws/cloudformation/openshift/images/3-complete.jpg" alt="complete-stack" /><p class="caption">complete-stack</p>
</div>
<ul>
<li><p>Run the script from the master instance's /root directory</p>
<p>(local-instance)# ssh root@ec2-<public-ip>.us-west-1.compute.amazonaws.com</p>
<p>(master-instance)# cd /root (master-instance)# ~/run.sh</p></li>
</ul>
<div class="figure">
<img src="https://github.com/savithruml/cloud-ops/blob/master/aws/cloudformation/openshift/images/4-run-sh.jpg" alt="run-stack" /><p class="caption">run-stack</p>
</div>
<ul>
<li>Once install is complete, login to the dashboards (WebUI) of both OpenShift &amp; Contrail. The URL's are listed in the <strong>Outputs</strong> tab of AWS CloudFormation</li>
</ul>
<div class="figure">
<img src="https://github.com/savithruml/cloud-ops/blob/master/aws/cloudformation/openshift/images/6-openshift-webui.png" alt="openshift-webui" /><p class="caption">openshift-webui</p>
</div>
<div class="figure">
<img src="https://github.com/savithruml/cloud-ops/blob/master/aws/cloudformation/openshift/images/7-contrail-webui.png" alt="contrail-webui" /><p class="caption">contrail-webui</p>
</div>
<ul>
<li><p>Verify all Contrail pods are running healthy, by logging into OpenShift &amp; Contrail dashboards</p>
<p><strong><em>OpenShift Dashboard &gt; My Projects &gt; kube-system &gt; Applications &gt; Pods</em></strong></p></li>
</ul>
<div class="figure">
<img src="https://github.com/savithruml/cloud-ops/blob/master/aws/cloudformation/openshift/images/8-contrail-pods.png" alt="contrail-pods" /><p class="caption">contrail-pods</p>
</div>
<pre><code>**_Contrail Dashboard &gt; Monitor &gt; Infrastructure &gt; Dashboard_**</code></pre>
<div class="figure">
<img src="https://github.com/savithruml/cloud-ops/blob/master/aws/cloudformation/openshift/images/9-contrail-status.png" alt="contrail-status" /><p class="caption">contrail-status</p>
</div>
<ul>
<li><p>Enable SNAT on the pod network, by logging into Contrail dashboard</p>
<p><strong><em>Contrail Dashboard &gt; Configure &gt; Networking &gt; Networks &gt; default-domain &gt; default&gt; k8s-default-pod-network (edit)</em></strong></p></li>
</ul>
<div class="figure">
<img src="https://github.com/savithruml/cloud-ops/blob/master/aws/cloudformation/openshift/images/10-enable-snat.jpg" alt="enable-snat" /><p class="caption">enable-snat</p>
</div>
<ul>
<li><p>Try the below labs</p>
<ol style="list-style-type: decimal">
<li><a href="https://s3-us-west-1.amazonaws.com/contrail-labs/usecase-1-openshift-build.pdf">LAB-1: Build/test/deploy highly scalable apps using OpenShift &amp; Contrail SDN</a></li>
<li><a href="https://s3-us-west-1.amazonaws.com/contrail-labs/usecase-2-openshift-ingress.pdf">LAB-2: Expose highly scalable apps using OpenShift &amp; Contrail SDN</a></li>
<li><a href="https://s3-us-west-1.amazonaws.com/contrail-labs/usecase-3-openshift-network-policy.pdf">LAB-3: Secure highly scalable apps using OpenShift &amp; Contrail SDN</a></li>
</ol></li>
</ul>
