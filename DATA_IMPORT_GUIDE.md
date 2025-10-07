# 📊 Data Import Guide

This guide explains how to import Delhi air quality data and custom CSV files into your Air Quality Monitoring System.

## 🚀 Quick Start

### Import Delhi Data (Recommended)

```bash
# Navigate to backend directory
cd backend

# Install dependencies
npm install

# Import Delhi data
npm run import:delhi
```

### Import Custom CSV

```bash
# Import your own CSV file
npm run import:data path/to/your/file.csv

# Clear existing data and import
npm run import:clear && npm run import:data path/to/your/file.csv
```

## 📁 Data Structure

### Delhi Data (Pre-loaded)

The system comes with a sample dataset containing 50+ Delhi air quality sources:

- **Traffic Sources**: Connaught Place, India Gate, Red Fort, etc.
- **Industrial Areas**: Okhla, Patparganj, Wazirabad, etc.
- **Construction Sites**: Anand Vihar, ITO, Metro Stations, etc.
- **Agricultural Areas**: Yamuna River Bank, Ridge Forest, etc.
- **Residential Areas**: Vasant Kunj, Saket, Gurgaon, etc.
- **Educational Institutions**: Delhi University, JNU, IIT Delhi, etc.
- **Transport Hubs**: Airport, Railway Stations, ISBTs, etc.
- **Power Plants**: Badarpur, Rajghat, Indraprastha, etc.

### CSV Format

Your CSV file should include these columns:

```csv
source_id,source_name,source_type,latitude,longitude,district,area,pm25,pm10,aqi,co,no2,so2,o3,last_updated,status
1,Connaught Place,Traffic,28.6315,77.2167,New Delhi,Central,85,142,142,1.2,45,28,65,2024-01-15 10:30:00,Active
```

#### Required Columns:
- `source_id`: Unique identifier
- `source_name`: Name of the pollution source
- `source_type`: Type (Traffic, Industrial, Construction, Agricultural, Residential, Educational, Transport, Power, Other)
- `latitude`: Latitude coordinate
- `longitude`: Longitude coordinate
- `district`: District name
- `area`: Area/locality name
- `pm25`: PM2.5 value (μg/m³)
- `pm10`: PM10 value (μg/m³)
- `aqi`: Air Quality Index
- `co`: Carbon Monoxide (ppm)
- `no2`: Nitrogen Dioxide (μg/m³)
- `so2`: Sulfur Dioxide (μg/m³)
- `o3`: Ozone (μg/m³)
- `last_updated`: Timestamp (YYYY-MM-DD HH:MM:SS)
- `status`: Active/Inactive

## 🛠️ Import Methods

### 1. Command Line Import

```bash
# Import Delhi data
npm run import:delhi

# Import custom CSV
npm run import:data data/your_file.csv

# Clear all data
npm run import:clear

# Clear and import
npm run import:clear && npm run import:delhi
```

### 2. API Import

#### Import Delhi Data
```bash
curl -X POST http://localhost:5000/api/import/delhi \
  -H "Content-Type: application/json" \
  -d '{"clearExisting": true}'
```

#### Import CSV File
```bash
curl -X POST http://localhost:5000/api/import/csv \
  -F "csvFile=@data/your_file.csv" \
  -F "clearExisting=true"
```

#### Check Import Status
```bash
curl http://localhost:5000/api/import/status
```

#### Clear All Data
```bash
curl -X DELETE http://localhost:5000/api/import/clear \
  -H "Content-Type: application/json" \
  -d '{"confirm": "true"}'
```

### 3. Admin Dashboard Import

1. Go to `/admin` in your browser
2. Login with admin credentials (admin/admin123)
3. Click on "Data Import" tab
4. Choose import method:
   - **Delhi Data**: Click "Import Delhi Data"
   - **Custom CSV**: Upload your CSV file and click "Import CSV File"

## 📊 Data Processing

### Air Quality Records

The system converts CSV data into AirQuality documents with:

- **Location**: GeoJSON Point with coordinates
- **Measurements**: PM2.5, PM10, AQI, CO, NO2, SO2, O3
- **Source Information**: Type, name, description
- **Metadata**: District, area, status, import timestamp
- **Timestamps**: Data collection and import times

### Smoke Reports

High AQI sources (AQI > 100) automatically generate smoke reports:

- **Severity**: Based on AQI levels
- **Source Type**: Mapped from CSV source_type
- **Visibility**: Based on AQI levels
- **Status**: Auto-verified
- **Tags**: Include source type and location

## 🔧 Configuration

### Environment Variables

```env
# MongoDB connection
MONGODB_URI=mongodb://localhost:27017/air_quality_db

# For production
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/air_quality_db
```

### Import Settings

- **Clear Existing**: Removes old data before import
- **Batch Size**: Processes data in batches
- **Error Handling**: Continues on duplicate keys
- **Validation**: Validates coordinates and data types

## 📈 Monitoring

### Import Status

Check the current data status:

```bash
curl http://localhost:5000/api/import/status
```

Response:
```json
{
  "success": true,
  "data": {
    "totalAirQualityRecords": 50,
    "totalSmokeReports": 25,
    "dataSources": [
      {"_id": "Delhi Air Quality Sources CSV", "count": 50},
      {"_id": "CSV Import", "count": 10}
    ]
  }
}
```

### Dashboard Monitoring

1. Go to Admin Dashboard (`/admin`)
2. Check the "Data Import" tab
3. View current data statistics
4. Monitor import progress

## 🐛 Troubleshooting

### Common Issues

1. **CSV Format Error**
   ```
   Error: Invalid CSV format
   ```
   - **Solution**: Check column headers match required format
   - **Fix**: Use the provided CSV template

2. **Coordinate Validation Error**
   ```
   Error: Invalid coordinates
   ```
   - **Solution**: Ensure latitude is between -90 and 90
   - **Solution**: Ensure longitude is between -180 and 180

3. **Database Connection Error**
   ```
   Error: MongoDB connection failed
   ```
   - **Solution**: Check MONGODB_URI environment variable
   - **Solution**: Ensure MongoDB is running

4. **Duplicate Key Error**
   ```
   Error: Duplicate key error
   ```
   - **Solution**: Use `--clear` flag to remove existing data
   - **Solution**: Check for duplicate source_id values

### Debug Commands

```bash
# Check MongoDB connection
mongo "mongodb://localhost:27017/air_quality_db" --eval "db.airqualities.count()"

# Check data in database
mongo "mongodb://localhost:27017/air_quality_db" --eval "db.airqualities.find().limit(5)"

# Check smoke reports
mongo "mongodb://localhost:27017/air_quality_db" --eval "db.smokereports.count()"
```

## 📋 Best Practices

### Data Preparation

1. **Validate Coordinates**: Ensure lat/lng are within valid ranges
2. **Check Data Types**: Ensure numeric fields are properly formatted
3. **Unique IDs**: Use unique source_id values
4. **Timestamps**: Use consistent date format (YYYY-MM-DD HH:MM:SS)
5. **File Size**: Keep CSV files under 10MB for web upload

### Import Process

1. **Backup Data**: Export existing data before clearing
2. **Test Import**: Use small test files first
3. **Monitor Progress**: Check logs during large imports
4. **Verify Results**: Check data in admin dashboard
5. **Clean Up**: Remove temporary files after import

### Performance

1. **Batch Size**: Large datasets are processed in batches
2. **Memory Usage**: Monitor memory during large imports
3. **Database Indexes**: Ensure proper indexes for queries
4. **Network**: Use local database for faster imports

## 🎯 Next Steps

After importing data:

1. **Verify Data**: Check admin dashboard for imported records
2. **Test Features**: Ensure all dashboard features work
3. **Monitor Performance**: Check database performance
4. **Update Charts**: Verify charts display new data
5. **Generate Reports**: Test smoke report generation

## 📞 Support

If you encounter issues:

1. Check the troubleshooting section above
2. Review import logs for specific errors
3. Verify CSV format matches requirements
4. Test with the provided Delhi dataset first
5. Check database connection and permissions

---

**Happy Importing! 📊**
