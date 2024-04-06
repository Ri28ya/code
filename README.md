# codeimport React, { useState } from 'react';
import './App.css'; // Import your CSS file here

function App() {
  const [formData, setFormData] = useState({
    name: '',
    email: '',
    birthday: '',
    phone: '',
    district: '', 
    lokSabha: '',
    legislativeAssembly: '', // Add legislativeAssembly field to formData
    regionType: '' // Add regionType field to formData
  });

  const districtsAndLokSabha = {
    // Define your districts and corresponding Lok Sabha constituencies here
    "Alappuzha": ["Alappuzha", "Mavelikkara"],
    "Ernakulam": ["Ernakulam", "Chalakudy"],
    "Idukki": ["Idukki"],
    "Kannur": ["Kannur", "Vadakara"],
    "Kasaragod": ["Kasaragod"],
    "Kollam": ["Kollam"],
    "Kottayam": ["Kottayam", "Pathanamthitta"],
    "Kozhikode": ["Kozhikode", "Vadakara"],
    "Malappuram": ["Malappuram", "Ponnani"],
    "Palakkad": ["Palakkad", "Alathur"],
    "Pathanamthitta": ["Pathanamthitta", "Mavelikkara"],
    "Thiruvananthapuram": ["Thiruvananthapuram", "Attingal"],
    "Thrissur": ["Thrissur", "Alathur"],
    "Wayanad": ["Wayanad"]
    // Add more districts and corresponding Lok Sabha constituencies
  };

  // Define legislative assembly options for each Lok Sabha constituency
  const legislativeAssemblyOptions = {
    "Alappuzha": {
      "Alappuzha": {
        "regions": [
          { name: "Alappuzha Panchayath", type: "Panchayath" },
          { name: "Ambalappuzha Panchayath", type: "Panchayath" },
          { name: "Cherthala Municipality", type: "Municipality" },
          { name: "Aroor Panchayath", type: "Panchayath" }
        ]
      },
      "Mavelikkara": {
        "regions": [
          { name: "Mavelikkara Municipality", type: "Municipality" },
          { name: "Chengannur Municipality", type: "Municipality" },
          { name: "Kunnathur Panchayath", type: "Panchayath" },
          { name: "Kuttanad Panchayath", type: "Panchayath" }
        ]
      }
    },
    "Ernakulam": {
      "Ernakulam":{
        "regions":[
          { name: "Ernakulam Municipality", type: "Municipality" },
          { name: "Thrikkakara Municipality", type: "Municipality" },
          { name: "Paravur Panchayath", type: "Municipality" },
          { name: "Kalamassery Panchayath", type: "Municipality" }
        ]
      },
      "Chalakudy": {
        "regions":[
          { name: "Chalakudy Municipality", type: "Municipality" },
          { name: "Kodungallur Municipality", type: "Municipality" },
          { name: "Perumbavoor Municipality", type: "Municipality" },
          { name: "Angamaly Municipality", type: "Municipality" }
        ]
      }
      // Define regions for Ernakulam Lok Sabha
    },
    // Define regions for other Lok Sabha constituencies
  };

  const handleChange = (e) => {
    const { name, value } = e.target;
    if (name === 'lokSabha') {
      setFormData(prevState => ({
        ...prevState,
        [name]: value,
        legislativeAssembly: '', // Reset legislative assembly when Lok Sabha is changed
        regionType: '' // Reset region type when Lok Sabha is changed
      }));
    } else {
      setFormData(prevState => ({
        ...prevState,
        [name]: value
      }));
    }
  };

  const handleSubmit = (e) => {
    e.preventDefault();
    // Handle form submission here
    console.log(formData);
  };

  return (
    <div className="container">
      <h1>Registration Form</h1>
      <form onSubmit={handleSubmit}>
        <div className="form-group">
          <label htmlFor="name">Name:</label>
          <input
            type="text"
            id="name"
            name="name"
            value={formData.name}
            onChange={handleChange}
            required
          />
        </div>
        <div className="form-group">
          <label htmlFor="email">Email:</label>
          <input
            type="email"
            id="email"
            name="email"
            value={formData.email}
            onChange={handleChange}
            required
          />
        </div>
        <div className="form-group">
          <label htmlFor="birthday">Birthday:</label>
          <input
            type="date"
            id="birthday"
            name="birthday"
            value={formData.birthday}
            onChange={handleChange}
            required
          />
        </div>
        <div className="form-group">
          <label htmlFor="phone">Phone:</label>
          <input
            type="tel"
            id="phone"
            name="phone"
            pattern="[0-9]{3}-[0-9]{3}-[0-9]{4}"
            value={formData.phone}
            onChange={handleChange}
            required
          />
          <small>Format: 123-456-7890</small>
        </div>
        <div className="form-group">
          <label htmlFor="district">District:</label>
          <select
            id="district"
            name="district"
            value={formData.district}
            onChange={handleChange}
            required
          >
            <option value="">Select a district</option>
            {Object.keys(districtsAndLokSabha).map((district, index) => (
              <option key={index} value={district}>{district}</option>
            ))}
          </select>
        </div>
        {/* Other form fields */}
        <div className="form-group">
          <label htmlFor="lokSabha">Lok Sabha:</label>
          <select
            id="lokSabha"
            name="lokSabha"
            value={formData.lokSabha}
            onChange={handleChange}
            required
          >
            <option value="">Select a Lok Sabha constituency</option>
            {formData.district && districtsAndLokSabha[formData.district].map((constituency, index) => (
              <option key={index} value={constituency}>{constituency}</option>
            ))}
          </select>
        </div>
        {formData.lokSabha && (
          <div className="form-group">
            <label htmlFor="legislativeAssembly">Legislative Assembly:</label>
            <select
              id="legislativeAssembly"
              name="legislativeAssembly"
              value={formData.legislativeAssembly}
              onChange={handleChange}
              required
            >
              <option value="">Select a Legislative Assembly</option>
              {formData.lokSabha && legislativeAssemblyOptions[formData.lokSabha] && Object.keys(legislativeAssemblyOptions[formData.lokSabha]).map((assembly, index) => (
                <optgroup key={index} label={assembly}>
                  {legislativeAssemblyOptions[formData.lokSabha][assembly].regions.map((region, index) => (
                    <option key={index} value={region.name}>{region.name} ({region.type})</option>
                  ))}
                </optgroup>
              ))}
            </select>
          </div>
        )}
        {formData.legislativeAssembly && (
          <div className="form-group">
            <label>Region Type:</label>
            <input
              type="text"
              value={formData.regionType}
              readOnly
            />
          </div>
        )}
        <button type="submit">Submit</button>
      </form>
    </div>
  );
}

export default App;
App.js
