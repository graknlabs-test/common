#
# Copyright (C) 2021 Vaticle
#
# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU Affero General Public License as
# published by the Free Software Foundation, either version 3 of the
# License, or (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU Affero General Public License for more details.
#
# You should have received a copy of the GNU Affero General Public License
# along with this program.  If not, see <https://www.gnu.org/licenses/>.
#
#

load("@vaticle_bazel_distribution//apt:rules.bzl", "assemble_apt", "deploy_apt")
load("@vaticle_bazel_distribution//common:rules.bzl", "assemble_targz")
load("@vaticle_dependencies//distribution:deployment.bzl", "deployment")
load("@vaticle_dependencies//tool/checkstyle:rules.bzl", "checkstyle_test")

exports_files(["typedb", "typedb.bat", "typedb.service"])

assemble_targz(
    name = "assemble-bash-targz",
    additional_files = {
        "//binary:typedb": 'typedb',
    },
    permissions = {
        "typedb": "0755",
    },
    visibility = ["//visibility:public"]
)

assemble_targz(
    name = "assemble-apt-targz",
    additional_files = {
        "//binary:typedb.service": "typedb.service",
    },
)

assemble_targz(
    name = "assemble-bat-targz",
    additional_files = {
        "//binary:typedb.bat": "typedb.bat",
    },
    visibility = ["//visibility:public"]
)

assemble_apt(
    name = "assemble-linux-apt",
    package_name = "typedb-bin",
    maintainer = "Vaticle <community@vaticle.com>",
    description = "TypeDB (binaries)",
    archives = [":assemble-bash-targz", ":assemble-apt-targz"],
    installation_dir = "/opt/typedb/core/",
    empty_dirs = [
        "var/log/typedb/",
    ],
    depends = [
        "openjdk-8-jre"
    ],
    permissions = {
        "var/log/typedb/": "0777",
    },
    symlinks = {
        "usr/local/bin/typedb": "/opt/typedb/core/typedb",
        "opt/typedb/core/server/logs": "/var/log/typedb/",
        "usr/lib/systemd/system/typedb.service": "/opt/typedb/core/typedb.service",
    },
)

deploy_apt(
    name = "deploy-apt",
    target = ":assemble-linux-apt",
    snapshot = deployment['apt.snapshot'],
    release = deployment['apt.release']
)

checkstyle_test(
    name = "checkstyle",
    include = glob(["*"]),
    license_type = "agpl",
)
