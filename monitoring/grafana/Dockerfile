FROM grafana/grafana:11.6.0

COPY datasources.yml /etc/grafana/provisioning/datasources/datasources.yml

# Dashboards are provisioned from disk so they are versioned in git and applied
# on redeploy. Alert rules and contact points deliberately are NOT provisioned
# here — they go in over the Grafana API with X-Disable-Provenance so thresholds
# stay tunable in the UI without a stack redeploy.
COPY provisioning/dashboards.yml /etc/grafana/provisioning/dashboards/dashboards.yml
COPY dashboards/ /var/lib/grafana/dashboards/
