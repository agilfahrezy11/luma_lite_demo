# Luma GE Frontend

Streamlit-based frontend interface for the Luma Geospatial engine land cover mapping application using Google Earth Engine Python API.

## Overview

This is the frontend interface for the Luma GE platform, which provides a user-friendly web interface for land cover mapping workflows. The backend algorithms and Earth Engine processing are handled by the separate [Luma-stack](https://github.com/epistem-io/luma-stack) repository.

## Features

- **Module 1**: Generate Image Mosaic - Acquisition of near cloud-free satellite imagery
- **Module 2**: Classification Scheme - Define land use/land cover classification schemes
- **Module 3**: Generate ROI - Sample data generation
- **Module 4**: Analyze ROI - Sample quality analysis and spectral plotting
- **Module 6**: Classification and LULC Creation - Generate land cover maps
- **Module 7**: Thematic Accuracy Assessment - Validate map accuracy

## Prerequisites

- Python 3.9 or higher
- Google Earth Engine account with authentication
- Google Cloud Console project with OAuth2 credentials (for authentication)
- Git

## Installation

### Option 1: Local Development Setup

1. **Clone the repository**:
   ```bash
   git clone <this-repository-url>
   cd luma-ge-frontend
   ```

2. **Create and activate virtual environment**:
   ```bash
   python -m venv venv
   source venv/bin/activate  # On Windows: venv\Scripts\activate
   ```

3. **Install dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

4. **Set up environment variables**:
   ```bash
   cp .env.example .env
   # Edit .env with your actual credentials
   ```

5. **Configure Google Earth Engine**:
   ```bash
   earthengine authenticate
   ```

6. **Run the application**:
   ```bash
   streamlit run home.py
   ```

### Option 2: Docker Deployment

1. **Set up secrets** (create these files with your actual credentials):
   ```bash
   mkdir -p secrets
   echo "your_base64_encoded_service_account_json" > secrets/service_account_b64.txt
   echo "your_base64_encoded_oauth_client_config" > secrets/oauth_client_config_b64.txt
   ```

2. **Build and run with Docker Compose**:
   ```bash
   docker-compose -f docker-compose.oauth.yml up --build
   ```

3. **Access the application** at `http://localhost:7860`

## Configuration

### Google Earth Engine Setup

1. Create a Google Cloud Console project
2. Enable the Earth Engine API
3. Create a service account and download the JSON key
4. Base64 encode the JSON key: `base64 -i service-account.json`

### OAuth2 Setup

1. In Google Cloud Console, create OAuth2 credentials
2. Set authorized redirect URIs to include your domain + `/auth/callback`
3. Download the client configuration JSON
4. Base64 encode the JSON: `base64 -i oauth-client-config.json`

### Environment Variables

Copy `.env.example` to `.env` and fill in your actual values:

- `GOOGLE_SERVICE_ACCOUNT_JSON_B64`: Base64 encoded service account JSON
- `GOOGLE_OAUTH_CLIENT_ID`: OAuth2 client ID
- `GOOGLE_OAUTH_CLIENT_SECRET`: OAuth2 client secret
- `GOOGLE_OAUTH_REDIRECT_URI`: OAuth2 redirect URI
- `OAUTH_COOKIE_SECRET_KEY`: Random secret key for session management

## Security Notes

⚠️ **IMPORTANT**: Never commit actual credentials to version control. Always use environment variables or Docker secrets for sensitive information.

## Backend Integration

This frontend depends on the `luma_ge` Python package from the [Luma-stack](https://github.com/epistem-io/luma-stack) repository. The backend package is automatically installed via the setup.py dependency.

## Development

### Project Structure

```
├── home.py                 # Main Streamlit application entry point
├── Pages/                  # Individual module pages
│   ├── 1_Module_1_Generate_Image_Mosaic.py
│   ├── 2_Module_2_Classification_scheme.py
│   ├── 3_Module_3_Generate_ROI.py
│   ├── 4_Module_4_Analyze_ROI.py
│   ├── 5_Module_6_Classification_and_LULC_Creation.py
│   ├── 6_Module_7_Thematic_Accuracy.py
│   └── 7_About.py
├── modules/                # Shared modules
│   └── nav.py             # Navigation component
├── .streamlit/            # Streamlit configuration
│   ├── config.toml        # Server and theme configuration
│   └── style.css          # Custom CSS styling
├── auth/                  # Authentication configuration
├── logos/                 # Application logos and images
├── ui_helper.py           # UI utility functions
├── requirements.txt       # Python dependencies
├── setup.py              # Package configuration
├── Dockerfile            # Docker build configuration
└── docker-compose.oauth.yml  # Docker Compose configuration
```

### Adding New Modules

1. Create a new page file in the `Pages/` directory
2. Follow the naming convention: `N_Module_N_Description.py`
3. Update the navigation in `modules/nav.py`
4. Import required functions from the `luma_ge` backend package

## Troubleshooting

### Common Issues

1. **Import errors for `luma_ge` package**: Ensure the backend package is installed correctly
2. **Earth Engine authentication errors**: Run `earthengine authenticate` and check service account setup
3. **OAuth errors**: Verify OAuth2 credentials and redirect URIs in Google Cloud Console
4. **Docker build failures**: Check that all required secrets files exist

### Getting Help

- Check the [Luma-stack repository](https://github.com/epistem-io/luma-stack) for backend-related issues
- Review Google Earth Engine [authentication guide](https://developers.google.com/earth-engine/guides/auth)
- Check Streamlit [documentation](https://docs.streamlit.io/) for frontend issues

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Contributing

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## Acknowledgments

- Built with [Streamlit](https://streamlit.io/)
- Powered by [Google Earth Engine](https://earthengine.google.com/)
- Uses [leafmap](https://leafmap.org/) and [geemap](https://geemap.org/) for interactive mapping