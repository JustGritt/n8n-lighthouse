# 🚦 Quick and easy n8n lighthouse report automation

An automated web performance monitoring solution that uses n8n workflows to generate Google Lighthouse reports for multiple websites on both desktop and mobile devices, storing results in MongoDB for analysis and tracking.

## 🎯 Features

- **Automated Performance Monitoring**: Scheduled Lighthouse audits for multiple websites
- **Multi-Device Testing**: Generates reports for both desktop and mobile experiences
- **Comprehensive Metrics**: Tracks performance, accessibility, best practices, and SEO scores
- **Data Persistence**: Stores all reports in MongoDB for historical analysis
- **Scalable Architecture**: Docker-based deployment with n8n workflow automation
- **Cloud Ready**: Configured for deployment on Fly.io

## 🏗️ Architecture

The system consists of:

- **n8n**: Workflow automation platform that orchestrates the Lighthouse testing
- **MongoDB**: Database for storing website URLs and performance reports
- **Google PageSpeed Insights API**: Powers the Lighthouse audits
- **Docker**: Containerized deployment for consistency across environments

## 📊 Workflow Overview

1. **Schedule Trigger**: Runs the workflow at defined intervals
2. **Fetch URLs**: Retrieves website URLs from MongoDB collection
3. **Generate Reports**: Creates Lighthouse reports for desktop and mobile
4. **Data Processing**: Formats and combines report data
5. **Store Results**: Saves performance metrics to MongoDB

## 🚀 Quick Start

### Prerequisites

- Docker and Docker Compose
- MongoDB instance (local or cloud)
- Google PageSpeed Insights API key

### Local Development

1. **Clone the repository**:
   ```bash
   git clone https://github.com/JustGritt/n8n-lighthouse.git
   cd n8n-lighthouse
   ```

2. **Set up environment variables**:
   ```bash
   # Update docker-compose.yml with your credentials
   N8N_BASIC_AUTH_USER=your_username
   N8N_BASIC_AUTH_PASSWORD=your_password
   ```

3. **Start the services**:
   ```bash
   docker-compose up -d
   ```

4. **Access n8n**:
   - Open http://localhost:5678
   - Login with your credentials
   - Import the workflow from `workflow/n8n-workflow.json`

### Cloud Deployment (Fly.io)

1. **Install Fly CLI**:
   ```bash
   curl -L https://fly.io/install.sh | sh
   ```

2. **Login and deploy**:
   ```bash
   fly auth login
   fly deploy
   ```

## ⚙️ Configuration

### MongoDB Setup

Create a MongoDB database with two collections:

**Links Collection** (`links`):
```json
{
  "country": "US",
  "affiliate": "example-site",
  "url": "https://example.com",
  "base_url": "https://example.com",
  "created_at": "2025-07-25",
  "desktop": {
    "url": "https://www.googleapis.com/pagespeed/v5/runPagespeed?url=https://example.com&strategy=desktop"
  },
  "mobile": {
    "url": "https://www.googleapis.com/pagespeed/v5/runPagespeed?url=https://example.com&strategy=mobile"
  }
}
```

**Reports Collection** (`reports`):
Results are automatically stored here with performance metrics.

### API Configuration

1. Get a Google PageSpeed Insights API key from [Google Cloud Console](https://console.cloud.google.com/)
2. Update the workflow with your API key in the HTTP Request nodes
3. Configure MongoDB connection credentials in n8n

## 📈 Data Structure

### Report Output Format

```json
{
  "country": "US",
  "affiliate": "example-site",
  "url": "https://example.com",
  "created_at": "2025-07-25",
  "desktop": {
    "performance": { "score": 0.95 },
    "accessibility": { "score": 0.88 },
    "best_practices": { "score": 0.92 },
    "seo": { "score": 0.90 }
  },
  "mobile": {
    "performance": { "score": 0.78 },
    "accessibility": { "score": 0.85 },
    "best_practices": { "score": 0.89 },
    "seo": { "score": 0.88 }
  }
}
```

## 🔧 Customization

### Modify Schedule
Edit the "Schedule Trigger" node in the workflow to change monitoring frequency:
- Hourly: `{ "hour": "*" }`
- Daily: `{ "hour": "9", "minute": "0" }`
- Weekly: `{ "weekday": "1", "hour": "9", "minute": "0" }`

### Add New Metrics
Extend the data formatting nodes to capture additional Lighthouse metrics:
- First Contentful Paint
- Largest Contentful Paint
- Cumulative Layout Shift
- Time to Interactive

### Custom Notifications
Add notification nodes to alert on performance regressions:
- Email notifications
- Slack integration
- Discord webhooks

## 🐛 Troubleshooting

### Common Issues

**API Rate Limits**:
- Enable retry logic with delays (already configured)
- Consider using multiple API keys for higher quotas

**Memory Issues**:
- Increase VM memory in `fly.toml` for large-scale monitoring
- Optimize workflow by processing sites in batches

**Database Connections**:
- Ensure MongoDB credentials are correctly configured
- Check network connectivity between services

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 🤝 Contributing

1. Fork the repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

## 📞 Support

For questions and support:
- Create an issue in this repository
- Check the [n8n documentation](https://docs.n8n.io/)
- Review [Google PageSpeed Insights API docs](https://developers.google.com/speed/docs/insights/v5/get-started)

---

🐢
