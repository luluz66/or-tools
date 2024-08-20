# Prefer generated BUILD files to be called BUILD over BUILD.bazel
# gazelle:build_file_name BUILD,BUILD.bazel
# gazelle:prefix github.com/or-tools/ortools/sat/cpmodel
load("@bazel_gazelle//:def.bzl", "gazelle")
load("@io_bazel_rules_go//go:def.bzl", "go_library", "go_test")

gazelle(name = "gazelle")

gazelle(
    name = "gazelle-update-repos",
    args = [
        "-from_file=go.mod",
        "-to_macro=deps.bzl%go_dependencies",
        "-prune",
    ],
    command = "update-repos",
)

go_library(
    name = "cpmodel",
    srcs = [
        "cp_model.go",
        "cp_solver.go",
        "domain.go",
    ],
    cdeps = [":cp_solver_c"],
    cgo = True,
    importpath = "github.com/or-tools/ortools/sat/cpmodel",
    visibility = ["//visibility:public"],
    deps = [
	"@com_github_golang_glog//:glog",
	"//ortools/sat:cp_model_go_proto",
	"//ortools/sat:sat_parameters_go_proto",
     "@org_golang_google_protobuf//proto",
	],
)

go_test(
    name = "cpmodel_test",
    srcs = [
        "cp_model_test.go",
        "cp_solver_test.go",
        "domain_test.go",
    ],
    embed = [":cpmodel"],
    deps = ["@com_github_golang_glog//:glog"],
)

cc_library(
    name = "cp_solver_c",
    srcs = ["cp_solver_c.cc"],
    hdrs = ["cp_solver_c.h"],
    deps = [
        "//ortools/sat:cp_model_cc_proto",
        "//ortools/sat:cp_model_solver",
        "//ortools/sat:sat_parameters_cc_proto",
        "//ortools/util:time_limit",
        "@com_google_absl//absl/log",
        "@com_google_absl//absl/status",
        "@com_google_absl//absl/strings",
    ],
)
