load("@bazel_gazelle//:def.bzl", "gazelle")

# gazelle:proto disable_global
# gazelle:exclude tools/deb-tools/
gazelle(
    name = "gazelle",
    external = "vendored",
    prefix = "sigs.k8s.io/etcd-manager",
)

load("//images:etcd.bzl", "supported_etcd_arch_and_version")

[
    genrule(
        name = "etcd-v{version}-linux-{arch}_{bin}".format(
            arch = arch,
            bin = bin,
            version = version,
        ),
        srcs = ["@etcd_{version}_{arch}_tar//file".format(
            arch = arch,
            bin = bin,
            version = version,
        )],
        outs = ["etcd-v{version}-linux-{arch}/{bin}".format(
            arch = arch,
            bin = bin,
            version = version,
        )],
        cmd = "tar -x -z --no-same-owner -f ./$(location @etcd_{version}_{arch}_tar//file) etcd-v{version}-linux-{arch}/{bin} && mv etcd-v{version}-linux-{arch}/{bin} \"$@\"".format(
            arch = arch,
            bin = bin,
            version = version,
        ),
        visibility = ["//visibility:public"],
    )
    for (arch, version) in supported_etcd_arch_and_version()
    for bin in [
        "etcd",
        "etcdctl",
    ]
]
