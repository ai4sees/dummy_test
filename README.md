# dummy_test
testing 
import numpy as np
import plotly.graph_objects as go
from plotly.subplots import make_subplots

# Generate synthetic time series data
np.random.seed(42)
n_points = 100
time = np.arange(n_points)
original_data = np.sin(0.1 * time) + 0.2 * np.random.randn(n_points)

# Add anomalies
anomalies_indices = [20, 40, 60, 80]
original_data[anomalies_indices] += 3.0 * np.random.randn(len(anomalies_indices))

# Apply some anomaly detection algorithm (e.g., simple thresholding)
threshold = 2.0
reconstruction_errors = np.abs(original_data - np.mean(original_data))
anomalies = reconstruction_errors > threshold

# Create subplots
fig = make_subplots(rows=3, cols=1, shared_xaxes=True, subplot_titles=['Original Time Series', 'Reconstruction Errors', 'Anomalies'])

# Plot original time series
fig.add_trace(go.Scatter(x=time, y=original_data, mode='lines', name='Original Data'), row=1, col=1)

# Plot reconstruction errors
fig.add_trace(go.Scatter(x=time, y=reconstruction_errors, mode='lines', name='Reconstruction Errors', line=dict(color='red')), row=2, col=1)
fig.add_trace(go.Scatter(x=time, y=np.full_like(reconstruction_errors, threshold), mode='lines', name='Threshold', line=dict(dash='dash')), row=2, col=1)

# Plot anomalies
fig.add_trace(go.Scatter(x=time, y=np.where(anomalies, original_data, None), mode='markers', marker=dict(color='red', size=8), name='Anomalies'), row=3, col=1)

# Update layout
fig.update_layout(title_text='Anomaly Detection with Reconstruction Errors', showlegend=False)

# Show the plot
fig.show()
