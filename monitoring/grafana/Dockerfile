FROM grafana/grafana:11.6.0

COPY datasources.yml /etc/grafana/provisioning/datasources/datasources.yml

# Dashboards are provisioned from disk so they are versioned in git and applied
# on redeploy. Alert rules and contact points deliberately are NOT provisioned
# here — they go in over the Grafana API with X-Disable-Provenance so thresholds
# stay tunable in the UI without a stack redeploy.
COPY provisioning/dashboards.yml /etc/grafana/provisioning/dashboards/dashboards.yml

# /etc/grafana, NOT /var/lib/grafana — the latter is where grafana-data is
# mounted, and a named volume mounts OVER whatever the image put there.
#
# Docker seeds a named volume from the image only when the volume is empty.
# grafana-data has existed since the stack was first deployed, long before these
# dashboards were added to the image, so the seeding never happened and these
# files were invisible at runtime. The provider read an empty directory, found
# nothing to provision, and every dashboard in Grafana stayed a stale copy from
# an old API push — reporting `provisioned: false` and dated months earlier.
#
# That is also why a newly generated dashboard could sit in this repo for months
# and simply never appear in Grafana.
#
# datasources.yml above was never affected: it lands in /etc/grafana, outside the
# volume. Same mechanism, different path — only the shadowed one broke.
COPY dashboards/ /etc/grafana/dashboards/
