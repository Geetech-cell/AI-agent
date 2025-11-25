# n8n Workflow Simulation Guide

## Overview
This n8n workflow simulates the three AI agents proposed for AutoParts Inc.:
1. **Quality Control Agent (QCA)** - Vision-based defect detection and root cause analysis
2. **Predictive Maintenance Agent (PMA)** - Equipment health monitoring and failure prediction
3. **Production Optimization Agent (POA)** - Dynamic scheduling and resource allocation

## Workflow Architecture

### Triggers
- **Schedule Trigger**: Runs every 5 minutes to poll production data
- **Webhook Trigger**: Receives real-time production events

### Agent Nodes

#### Quality Control Agent (QCA)
- **Input**: Production line data with defect rates
- **Processing**: 
  - Vision analysis simulation
  - Root cause identification (temperature, tool wear, material variance)
  - Recommendation generation
- **Output**: Quality alerts with 94% confidence scores
- **Actions**: Store in database, send Slack alerts for high defect rates

#### Predictive Maintenance Agent (PMA)
- **Input**: Equipment sensor data (vibration, temperature, acoustic)
- **Processing**:
  - Health score calculation (0-100)
  - Failure probability prediction
  - Remaining Useful Life (RUL) estimation
  - Maintenance action determination
- **Output**: Maintenance predictions with priority levels
- **Actions**: Store predictions, send critical alerts (>70% failure probability)

#### Production Optimization Agent (POA)
- **Input**: Customer orders with customization requirements
- **Processing**:
  - Multi-objective optimization (cost, time, quality)
  - Dynamic line assignment
  - Shift optimization
  - Material allocation
- **Output**: Optimized schedules with 22% throughput improvement
- **Actions**: Update production schedule, refresh dashboard

### Data Flow
```
Production Data → Three Parallel Agents → Database Storage → Alerts/Dashboard
```

## Setup Instructions

### Prerequisites
- n8n instance (self-hosted or cloud)
- PostgreSQL database (optional, for data storage)
- Slack workspace (optional, for alerts)

### Installation Steps

1. **Import Workflow**
   - Open n8n interface
   - Click "Import from File"
   - Select `AutoParts_AI_Agent_System.json`

2. **Configure Credentials** (if using real integrations)
   - PostgreSQL: Add database connection details
   - Slack: Add workspace OAuth token
   - HTTP Auth: Add API authentication headers

3. **Activate Workflow**
   - Click "Active" toggle in top-right
   - Workflow will start running on schedule

### Testing the Workflow

#### Option 1: Manual Execution
1. Click "Execute Workflow" button
2. Observe data flow through all nodes
3. Check execution logs for agent outputs

#### Option 2: Webhook Testing
Send a POST request to the webhook URL:
```bash
curl -X POST https://your-n8n-instance/webhook/production-event \
  -H "Content-Type: application/json" \
  -d '{
    "lineId": "Line-A",
    "defectRate": 0.08,
    "temperature": 92,
    "vibration": 8.5,
    "toolWear": 0.78,
    "machineId": "CNC-001",
    "operatingHours": 4800,
    "orderId": "ORD-5432",
    "customization": true,
    "priority": "HIGH",
    "leadTime": 14,
    "estimatedCost": 6500
  }'
```

## Simulated Data Examples

### Quality Control Agent Output
```json
{
  "timestamp": "2025-11-25T08:53:17.000Z",
  "lineId": "Line-A",
  "defectRate": 0.08,
  "defectTypes": {
    "dimensional": 0.035,
    "surface": 0.028,
    "material": 0.017
  },
  "rootCause": "Temperature deviation",
  "recommendation": "Adjust cooling system, reduce cycle time by 8%",
  "confidence": 0.94
}
```

### Predictive Maintenance Agent Output
```json
{
  "timestamp": "2025-11-25T08:53:17.000Z",
  "machineId": "CNC-001",
  "healthScore": 67.3,
  "failureProbability": 0.73,
  "remainingUsefulLife": "850 hours",
  "sensorReadings": {
    "vibration": 8.5,
    "temperature": 92,
    "acousticSignature": 78.3
  },
  "maintenanceAction": "URGENT: Schedule immediate maintenance",
  "priority": "CRITICAL",
  "estimatedDowntime": "4 hours"
}
```

### Production Optimization Agent Output
```json
{
  "timestamp": "2025-11-25T08:53:17.000Z",
  "orderId": "ORD-5432",
  "customerPriority": "HIGH",
  "isCustom": true,
  "originalLeadTime": 14,
  "originalCost": 6500,
  "optimizedLeadTime": 8,
  "optimizedCost": 5720,
  "assignedLine": "Line-C",
  "assignedShift": "Shift-2 (Standard)",
  "throughputImpact": "+22%",
  "utilizationRate": "87%",
  "onTimeDeliveryProb": 0.95
}
```

## Key Features Demonstrated

### 1. Parallel Agent Processing
All three agents process data simultaneously, demonstrating real-time multi-agent coordination.

### 2. Intelligent Decision Making
- QCA identifies root causes and generates actionable recommendations
- PMA predicts failures before they occur with priority-based alerts
- POA optimizes schedules considering multiple constraints

### 3. Integration Points
- Database storage for historical analysis
- Slack notifications for critical events
- Dashboard updates for real-time monitoring
- Webhook support for event-driven processing

### 4. Realistic Simulation Logic
Each agent includes sophisticated algorithms:
- Health score calculations based on multiple sensor inputs
- Failure probability using weighted risk factors
- Multi-objective optimization for scheduling
- Root cause analysis with confidence scoring

## Customization Options

### Adjust Thresholds
Modify the IF nodes to change alert thresholds:
- Defect rate threshold (currently 5%)
- Failure probability threshold (currently 70%)

### Add More Agents
Extend the workflow with additional specialized agents:
- Energy Optimization Agent
- Supply Chain Agent
- Worker Safety Agent

### Enhance Data Sources
Replace simulated data with real integrations:
- IoT sensor platforms (ThingWorx, AWS IoT)
- MES systems (SAP ME, Siemens Opcenter)
- Vision systems (Cognex, Keyence)

## Performance Metrics

The workflow demonstrates the expected improvements:
- **Defect Reduction**: From 15% to 3% (simulated in QCA logic)
- **Downtime Prevention**: 70% reduction (PMA failure prediction)
- **Lead Time Improvement**: 40% reduction for custom orders (POA optimization)
- **Cost Savings**: 12% reduction through optimized scheduling

## Troubleshooting

### Common Issues
1. **Workflow not executing**: Check if workflow is activated
2. **Database errors**: Verify PostgreSQL credentials and table schemas
3. **Slack alerts not sending**: Confirm Slack app permissions and channel access
4. **Webhook not responding**: Check webhook URL and firewall settings

### Debug Mode
Enable debug mode to see detailed execution logs:
1. Click on any node
2. View "Output" tab
3. Inspect JSON data structure

## Next Steps

1. **Deploy to Production n8n Instance**: Use n8n Cloud or self-hosted server
2. **Connect Real Data Sources**: Replace mock data with actual production systems
3. **Implement Database Schema**: Create tables for quality_alerts, maintenance_predictions, production_schedule
4. **Configure Monitoring**: Set up n8n execution monitoring and error handling
5. **Scale Horizontally**: Add more agent instances for different production lines

## Architecture Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                     Production Data Sources                  │
│              (Sensors, Vision Systems, MES, ERP)            │
└────────────────────┬────────────────────────────────────────┘
                     │
                     ▼
┌─────────────────────────────────────────────────────────────┐
│                      n8n Workflow Engine                     │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐     │
│  │     QCA      │  │     PMA      │  │     POA      │     │
│  │  (Quality)   │  │ (Maintenance)│  │(Optimization)│     │
│  └──────┬───────┘  └──────┬───────┘  └──────┬───────┘     │
│         │                  │                  │              │
│         └──────────────────┼──────────────────┘              │
│                            │                                 │
└────────────────────────────┼─────────────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────────┐
│                    Output & Actions                          │
│  • Database Storage  • Slack Alerts  • Dashboard Updates    │
└─────────────────────────────────────────────────────────────┘
```

## Conclusion

This n8n workflow provides a functional simulation of the AI agent system proposed for AutoParts Inc. It demonstrates the core capabilities of each agent type and shows how they work together to improve manufacturing operations.

For production deployment, this workflow would be enhanced with:
- Real sensor integrations
- Machine learning model endpoints
- Advanced error handling
- Scalability configurations
- Security hardening
