
- [OpenHPC 에서 공개한 첫 고성능 컴퓨팅용 소프트웨어 패키지 OpenHPC Releases Latest Stack with 60-plus Packages](https://scienceon.kisti.re.kr/srch/selectPORSrchTrend.do?cn=GTB2016000426)
- [Comparison_of_cluster_software](https://en.wikipedia.org/wiki/Comparison_of_cluster_software)
---
- [github-브런치를 1.39GA로 변경해서 볼것, 상단 메뉴의 wiki 볼것](https://github.com/openhpc/ohpc)
- OpenHPC provides pre-built binaries via repositories for use with standard Linux package manager tools (e.g. yum or zypper).
- pre-built (O)
- Source compile (X)
- Computing 속도 보다는 운영 관리쪽???
---
- [OpenHPC 비전, 미션](https://openhpc.community/about-us/vision/)

---
# OpenHPC로 배포(provisioning)하면 OS의 기본패키지와 업데이트시 충돌가능성이 있어 보임(분석 필요)

---
## 용어

- system management server (SMS)

---
- [Kubernetes, HPC and MPI](https://www.stackhpc.com/k8s-mpi.html)

    - There are projects underway with the goal of integrating Kubernetes with MPI. One notable approach, kube-openmpi, uses Kubernetes to launch a cluster of containers capable of supporting the target application set. Once this Kubernetes namespace is created, it is possible to use kubectl to launch and mpiexec applications into the namespace and leverage the deployed OpenMPI environment. (kube-openmpi only supports OpenMPI, as the name suggests).
    - Another framework, Kubeflow, also supports execution of MPI tasks atop Kubernetes. Kubeflow’s focus is evidence that the driving force for MPI-Kubernetes integration will be large-scale machine learning. Kubeflow uses a secondary scheduler within Kubernetes, kube-batch to support the scheduling and uses OpenMPI and a companion ssh daemon for the launch of MPI-based jobs.

- [kube-openmpi: Open MPI jobs on Kubernetes](https://github.com/everpeace/kube-openmpi)

```sh
$ kubectl -n $KUBE_NAMESPACE exec -it $MPI_CLUSTER_NAME-master -- mpiexec --allow-run-as-root \
  --hostfile /kube-openmpi/generated/hostfile \
  --display-map -n 4 -npernode 1 \
  sh -c 'echo $(hostname):hello'
 Data for JOB [43686,1] offset 0

 ========================   JOB MAP   ========================

 Data for node: MPI_CLUSTER_NAME-worker-0        Num slots: 2    Max slots: 0    Num procs: 1
        Process OMPI jobid: [43686,1] App: 0 Process rank: 0 Bound: UNBOUND

 Data for node: MPI_CLUSTER_NAME-worker-1        Num slots: 2    Max slots: 0    Num procs: 1
        Process OMPI jobid: [43686,1] App: 0 Process rank: 1 Bound: UNBOUND

 Data for node: MPI_CLUSTER_NAME-worker-2        Num slots: 2    Max slots: 0    Num procs: 1
        Process OMPI jobid: [43686,1] App: 0 Process rank: 2 Bound: UNBOUND

 Data for node: MPI_CLUSTER_NAME-worker-3        Num slots: 2    Max slots: 0    Num procs: 1
        Process OMPI jobid: [43686,1] App: 0 Process rank: 3 Bound: UNBOUND

 =============================================================
MPI_CLUSTER_NAME-worker-1:hello
MPI_CLUSTER_NAME-worker-2:hello
MPI_CLUSTER_NAME-worker-0:hello
MPI_CLUSTER_NAME-worker-3:hello
```


## 프로비저닝(warewulf)

```
# wwmkchroot
ERROR: Missing VNFS template name

/usr/bin/wwmkchroot [options] TEMPLATE_NAME PATH

OPTIONS:
    -d        Debug output
    -g        Disable install GPG checks
    -v        Verbose output
    -h        Show usage

TEMPLATE_NAME (select one of the following):
   * centos-6             A clone of Red Hat Enterprise Linux 6
   * centos-7             A clone of Red Hat Enterprise Linux 7
   * centos-8             Red Hat Enterprise Linux 8
   * debian7-32           A base 32 bit Debian wheezy chroot
   * debian7-64           A base 64 bit Debian wheezy chroot
   * debian-8             A base 64 bit Debian jessie chroot
   * golden-system        Build a local chroot from a golden system
   * opensuse-15.1        OpenSUSE LEAP 15.1
   * opensuse-15.2        OpenSUSE LEAP 15.2
   * opensuse-42.3        OpenSUSE LEAP 42.3
   * opensuse-tumbleweed  OpenSUSE Tumbleweed Rolling Release
   * sl-7                 Scientific Linux 7
   * sles-12              SUSE Linux Enterprise Server 12
   * sles-15              SUSE Linux Enterprise Server 15 Update 1
   * ubuntu-16.04         A base 64 bit Ubuntu xenial chroot
   * ubuntu-18.04         A base 64-bit Ubuntu cosmic chroot

PATH:
   This is the location where the VNFS will be created

EXAMPLES:

 # wwmkchroot rhel-generic /var/chroots/rhel
 # wwmkchroot debian8-64 /var/chroots/deb8

출처: https://minimonk.net/10347 [구차니의 잡동사니 모음]
```


- [Use Kubernetes with OpenHPC(warewulf)](http://ami-gs.github.io/openhpc/kubernetes/2018/02/05/openHPC-w-k8s.html)
    - Centos7.4 + k8s

설치
[Running Highly Available Clusters with Kubeadm](https://www.weave.works/blog/running-highly-available-clusters-with-kubeadm)